## 简历内容

* 监控系统: 负责实时告警模块开发，支持自定义规则配置，实现终端通知、邮件推送。 
* 高可用部署: 开发堡垒机HA集群部署功能，提高堡垒机平台可用性 
* CLI 工具: 增强运维命令行工具，开发调用HA部署，节点健康检查等功能。 
* 审计系统: 优化日志架构，实现HA环境下的跨实例日志同步、持久化存储和高效查询。

### 监控系统：告警模块

* 背景：
  * 在堡垒机平台中需要支持，若系统中的服务或组件如audit，core，es，redis等组件宕机。在系统页面提示出现红色告警信息，同时配置支持邮件和终端通知。

* 实现
  * **告警收集**
    * **监控指标**
      * 业务服务：core、audit
        * 业务服务暴露/health 接口：若返回200 则业务正常；500或超时则异常
      * 中间件：
        * es
          * GET _cluster/health 接口
          * 返回GREEN/YELLOW/RED
          * PS[[es中的状态代表什么？]]
        * redis
          * redis-cli PING 命令
          * 返回PONG 则正常
          * 不响应则触发告警
        * mysql
          * 使用mysqladmin ping 来检测
        * Prometheus
          * GET /-/ready
            * 返回200 OK则正常
            * 返回500或其他则告警
    * **监控运行**
      * cron每10s 检查服务状态
      * 将检查到的服务状态存到redis
        * redis使用hset来存状态
        * 为什么要存到redis？
          * 去抖动：
            * 在健康检查过程中，由于网络延迟、瞬时负载波动或系统短暂异常，可能会出现短时间内服务状态从健康变为不健康后又迅速恢复的情况。如果对每一次的检测结果都立即发送告警，就会产生大量重复、误导性的告警信息，给运维人员带来困扰。去抖动的目的是只在故障状态持续一段时间后才触发告警，从而**过滤掉这些短暂的波动**。
            * 选择连续检测确认：
              * 三遍及以上才发送告警
            * 存到redis优势
              * 可以对比当前状态与上一次检测结果决定是否触发告警（存本地也可以啊）
              * 汇聚多个节点的数据，形成一个全局视图，方便整体监控和统一决策
              * 扩展性：如果后续需要引入更多的监控项或动态调整健康检查策略，集中存储使得系统扩展和修改更加集中和统一，降低各节点间的配置和实现差异。
    * **告警触发并发送**
      * 发送目标
        * 前台页面
        * 邮件通知
        * 终端通知
      * 发送流程
        * 每次把状态存到redis后，遍历状态，若有down的，则发送redis 广播
          * ws订阅redis广播所以可以直接反馈在前台
          * 终端订阅广播所以可以直接显示在终端
        * 集成SMTP协议，当遍历到down时，发送告警邮件

#### 问题

* **这里邮件发送是怎么做的？**
  * 邮件发送集成了SMTP协议以及邮件发送组件，SPpringBoot提供了JAvaMailSender，在配置文件中配置一下发送参数，springbbot就会自动创建一个JavaMailSender的Bean；再定一个一个邮件通知服务，在告警的方法里使用一下即可。
* **这里考虑异步发送，如果发送失败会怎么样？**
  * 异常：首先异步的方法里会捕获所有异常，记录错误日志，方便后续排查
  * 重试：使用Spring Retry或者自定义重试逻辑
  * 备用：因为邮件发送始终失败，还有其他方式，比如会有和终端会有通知也可以及时看到信息；
* **如何利用redis实现状态存储以及去抖动的？**
  * redis存什么
    * 首先redis在这个系统中使用hset命令存储各个服务的最新状态信息；也就是说kv分别对应服务名和状态以及时间戳
  * 去抖动的步骤
    * 存储：每次健康检查，检测结果会立即存入redis
    * 对比：每一次检测系统会将当前状态与上次状态进行对比
    * 避免误告警：
      * 连续检测确认：如果连续三次或更多次检测均为“down”

* **若三次检测均为down告警了以后，服务马上恢复了呢？**
  * 有恢复通知的机制
    * 若下三次连续检测为up的话，才会发送恢复通知
* **这里使用的什么数据结构呢？**
  * concurrentHashMap
  * 可能只有发送邮件或这告警的时候才会有并发读，所以是否使用concurrentHashMap其实没什么所谓
* **你是怎么设计对prometheus的健康监测的**？
  * 实际上就是对 Prometheus 的 `/-/ready` 端点发送 HTTP 请求。如果返回状态码为 200，则返回 `true`（健康）；否则返回 `false`（不健康）
* **有扩展性考虑吗？**
  * **重试机制**：在接口实现中可以内置重试逻辑（例如使用 Spring Retry），对于偶发性网络故障进行重试判断，减少误判。
  * **配置化**：可以将判断健康的阈值、重试次数、超时时间等参数配置化，便于根据不同环境和需求进行调整。
* **你会如何实现这个配置化？**
  * 定义配置文件
  * 创建配置类
    * `@ConfigurationProperties` 注解创建一个配置类，将配置项绑定到 Java 对象上
  * 注入配置到HealthChecker实现类
* `@ConfigurationProperties`  和`@Configuration`**有啥区别？** 
  * `@ConfigurationProperties` 
    * 用来将配置文件中的属性绑定到java bean上的，实现配置的外部化和集中管理
  * `@Configuration`
    * 用于标注一个类作为Spring的配置类，相当于传统XML配置文件，主要用于定义和转个屁Bean
    * 特性
      * 通常会包含多个@Bean方法，这些方法返回的对象会被注册为Spring容器中的Bean 

* **你是如何保证不同发送渠道的通知是幂等的**（即每次状态变化只触发一次通知）？
  * 由于所有通知都通过同一入口处理，这保证了在单次检测循环中，基于同一判断条件（状态是否改变）只会触发一次通知。



### 审计系统

* 背景
  * 在堡垒机系统中，审计系统主要负责对用户操作进行详细记录和监控。它可以实时记录用户登录、执行的命令、文件传输等敏感操作，并生成详细的日志。

### 高可用HA部署

* 流程

  * 渲染模板文件和脚本
  * 文件分发
  * 脚本执行
  * 返回结果

  ### 问题

  * **你这个HA部署的业务流程是怎么样的？**
    * 这个过程主要分为5个阶段：
      * 渲染模板文件和脚本
        * 所有运行堡垒机系统的节点（A 和 B）在系统安装时就包含了这些模板文件
          * Keepalived 配置模板
          * Docker Compose 文件模板
          * ha_deploy.sh.tpl
        * 根据前端传入的vip 和两个node的ip，渲染出模板文件和部署脚本
          * Keepalived 配置文件
          * Docker Compose 文件
          * 部署脚本ha_deploy.sh
            * 部署脚本中包含启动keepalived服务的命令
      * 检查版本
        * 建立SSH链接，检查AB两个服务器中服务版本是否一致
      * 文件分发
        * 通过SCP把文件分发到B-node中
      * 脚本执行
        * 两个node根据脚本自动执行配置更新，密钥交换等操作
      * 返回结果
        * 返回结果给前端
  * **为什么通过vip就可以访问从节点呢？**
    * 在HA部署中，使用了Keepalived工具来监控主节点的健康状态
      * 主节点出现故障，VIP会漂移到从节点
  * **什么是keepalived？你知道他的工作原理吗？**
    * 这是一个基于Linux的高可用服务，主要用于实现 VRRP（Virtual Router Redundancy Protocol，虚拟路由冗余协议）。VRRP是一个容错协议，用于在多个路由器之间自动选举出一个主设备，以对外提供服务，其余设备作为备份
    * 广告消息
      * 每个参与VRRP的节点会定期发送VRRP广告消息，详细会通过组播或广播来发送到局域网内的所有节点，发送的消息内容（WRID、priority、interval..）
    * 选举过程
      * 节点启动
        * 所有节点定时发送或监听VRRP广告消息
      * 广告消息发送
        * 主节点（初始为默认配置的优先级）以固定间隔发送广告消息
      * 监听
        * back节点监听广告消息，规定时间（3倍广告间隔）未收到，则会认为master失效
      * 选举
        * back没有收到master广告时，所有back节点参与选举
          * 优先级比较：优先级高的成为新的master
          * 平级：优先级相同IP地址较高的为Master
          * 动态调整：通过track_script机制，健康检查不通过可以降低优先级
  * **你提到的组播和广播是什么？**
    * 广播是将数据包发送到网络中所有设备
    * 组播是将数据包发送给一个特定的组，只有加入该组的设备才能接收到数据
      * 网络设备（例如服务器或路由器）使用 Internet Group Management Protocol（IGMP）来向局域网内的路由器或交换机声明，它希望接收发送到某个特定组播地址（例如 224.0.0.18）的数据包。这个声明就相当于“加入”了这个组播组。
        
  * **track_script机制是什么？**
    * 通过定时执行外部脚本并根据返回值调整节点权重，使 Keepalived 能够根据实时状态自动决定 VIP 的归属
    * 比如指定的脚本返回0表示健康，非0为出现问题，就动态将当前节点的权重减少
  * **你的部署脚本做了哪些工作**？
    * 备份旧的 Keepalived 和 Docker Compose 配置文件 
    * 更新/写入新配置文件
      * （Keepalived 配置和 Docker Compose 文件）
      * **Elasticsearch 配置**：更新 ES 配置文件（如 `/etc/elasticsearch/elasticsearch.yml`），设置索引副本数、节点角色等参数
      * **Redis 配置**：更新 Redis 配置文件（如 `/etc/redis/redis.conf`），确保 `replicaof` 或 `replicaof` 参数正确设置，以实现主从复制
      * **PostgreSQL 配置**：更新 PostgreSQL 配置文件（如 `postgresql.conf` 和 `pg_hba.conf`），配置 `wal_level`、`max_wal_senders`、`hot_standby` 等参数，支持流复制。
    * 执行密钥交换（建立节点间安全通信，确保数据同步）
    * 重启 Keepalived 服务以应用新配置 
    * 启动 Docker Compose 部署的业务容器
  * **数据同步是怎么做的呢？比如主节点坏了切换到副节点还会保留用户的运行信息吗？**

  ```shell
  # --- 更新 PostgreSQL 配置 ---
  TEMPLATE_PG="/app/resources/templates/postgresql.conf.tpl"
  TARGET_PG="/etc/postgresql/12/main/postgresql.conf"
  if [ "$NODE_ROLE" == "master" ]; then
      # 主节点：开启复制相关参数，但无需启用只读模式
      sed "s/\${WAL_LEVEL}/replica/g; s/\${MAX_WAL_SENDERS}/10/g; s/\${HOT_STANDBY}/off/g" "$TEMPLATE_PG" > "$TARGET_PG"
  else
      # 副节点：开启 hot_standby 用于流复制
      sed "s/\${WAL_LEVEL}/replica/g; s/\${MAX_WAL_SENDERS}/5/g; s/\${HOT_STANDBY}/on/g" "$TEMPLATE_PG" > "$TARGET_PG"
  fi
  log "更新 PostgreSQL 配置文件至 $TARGET_PG"
  
  # 更新 pg_hba.conf 添加复制规则（通用配置，实际可调整 IP 范围和认证方式）
  echo "host replication replicator 0.0.0.0/0 md5" >> /etc/postgresql/12/main/pg_hba.conf
  log "更新 pg_hba.conf 添加复制规则"
  
  # --- 更新 Redis 配置 ---
  TEMPLATE_REDIS="/app/resources/templates/redis.conf.tpl"
  TARGET_REDIS="/etc/redis/redis.conf"
  if [ "$NODE_ROLE" == "master" ]; then
      # 主节点不需要 replicaof 参数
      sed "s/\${REPLICAOF}//g" "$TEMPLATE_REDIS" > "$TARGET_REDIS"
  else
      # 副节点：设置 replicaof 指向主节点
      sed "s/\${REPLICAOF}/replicaof 192.168.1.100 6379/g" "$TEMPLATE_REDIS" > "$TARGET_REDIS"
  fi
  log "更新 Redis 配置文件至 $TARGET_REDIS"
  
  # --- 更新 Elasticsearch 配置 ---
  TEMPLATE_ES="/app/resources/templates/elasticsearch.yml.tpl"
  TARGET_ES="/etc/elasticsearch/elasticsearch.yml"
  # 模板中可包含 ${CLUSTER_NAME} 和 ${NODE_ROLE}
  sed "s/\${CLUSTER_NAME}/ha_cluster/g; s/\${NODE_ROLE}/$NODE_ROLE/g" "$TEMPLATE_ES" > "$TARGET_ES"
  log "更新 Elasticsearch 配置文件至 $TARGET_ES"
  
  ```

  * pg

    * master
      * `postgresql.conf` 配置
        * **`wal_level = replica`**
          主节点需要生成足够详细的 WAL 日志，以便从节点复制数据。
        * **`max_wal_senders`**
          设置为较高值（例如 10 或更高），允许多个从节点同时连接进行复制。
        * **`hot_standby`**
          通常主节点不需要开启只读查询模式，此项可以保持默认（或设置为 off），因为主节点本身处理写入操作。
      * **`pg_hba.conf` 配置：**
        - `host replication replicator 192.168.1.0/24 md5`
        - 允许复制用户（例如 replicator）从网络中连接到主节点的规则
    * Slave
      * `postgresql.conf` 配置：
        *  `wal_level = replica`
          * （在从节点上这个参数也必须与主节点保持一致）。
        * **`hot_standby = on`**
          从节点开启只读查询模式，允许在复制过程中提供查询服务。

  * redis

    * Master

      * redis.conf

        * 主节点不需要包含 `replicaof` 指令，因为它自己处理所有写操作。

        - 可配置持久化（RDB/AOF）和安全认证（例如 `requirepass`）等参数，但在复制方面无需指定从节点信息。

    * slave

      * redis.conf

        * 表示该节点作为从节点，从主节点（IP：192.168.1.100、端口：6379）复制数据。

        * ```
          replicaof 192.168.1.100 6379
          ```

  * es

    * master

      * elasticsearch.yml 
        * 配置 `node.master: true` 以及 `node.data: true`（如果该节点同时承担数据存储角色）。
        * 设置集群名称（例如 `cluster.name: ha_cluster`），并确保 `discovery.seed_hosts` 列出所有集群内主要节点的 IP。
        * 主节点不需要单独指定“复制”指令，因为索引的副本数通常通过索引设置进行配置。

    * slave

      * 可以配置为 `node.master: false`，而 `node.data: true`，让它专注于存储和查询服务。

        同样配置 `cluster.name` 和 `discovery.seed_hosts`，确保能加入集群。

* **主从切换时候呢，这三个中间件也要进行主从切换**
  * pg
    * 副节点需要执行 `pg_ctl promote`（或使用工具如repmgr、Patroni）将自身提升为主节点。这意味着从节点停止只读模式，开始接受写操作。
  * redis
    * 新主节点的配置中不再包含 `replicaof` 指令，而其余从节点则会自动接收到新主节点的IP和端口信息，从而重新建立复制连接，确保数据同步不中断。
  * es
    * 集群内自动选举新主节点，重新分配分片和副本
* **运维强制主从切换呢？**

```shell
#!/bin/bash
# force_switch.sh - 强制主从切换示例脚本
# 此脚本用于在 HA 层面触发 PostgreSQL、Redis 和 Elasticsearch 的主从切换
# 并触发 Keepalived 的 VIP 漂移。以下仅为伪代码示例。

echo "开始强制主从切换..."

# 1. 对 PostgreSQL 执行 promote 操作，将从节点提升为主节点
echo "执行 PostgreSQL promote 操作..."
pg_ctl promote -D /var/lib/postgresql/12/main
if [ $? -ne 0 ]; then
    echo "PostgreSQL promote 失败"
    exit 1
fi

# 2. 对 Redis 调用 Sentinel API 触发故障转移（示例：使用 curl）
echo "调用 Redis Sentinel 进行主从切换..."
curl -X POST http://localhost:26379/switch-master -d '{
   "name": "mymaster",
   "ip": "192.168.1.100",
   "port": "6379"
}'
if [ $? -ne 0 ]; then
    echo "Redis Sentinel 切换失败"
    exit 1
fi

# 3. 对 Elasticsearch 检查集群健康并确认新主节点（示例调用API）
echo "检查 Elasticsearch 集群健康..."
curl -X GET "http://localhost:9200/_cluster/health?pretty"
if [ $? -ne 0 ]; then
    echo "Elasticsearch 健康检查失败"
    exit 1
fi

# 4. 触发 Keepalived 重启以完成 VIP 漂移
echo "重启 Keepalived 服务..."
systemctl restart keepalived
if [ $? -ne 0 ]; then
    echo "Keepalived 重启失败"
    exit 1
fi

echo "强制主从切换操作完成"
exit 0

```

### CLI开发

* 调用HA部署
  * rest api
* 节点健康检查
  * 调用实际的健康检查函数
  * 就是将各个服务的健康检查 REST API 调用结果聚合起来，形成一个整体的健康状态报告。这样你可以一次性获得各个中间件（比如 PostgreSQL、Redis、Elasticsearch 以及其它业务服务）的健康状态，而不是单独去调用每个服务的接口。

#### 问题

* **如何使用go开发的CLI工具**？
  * 使用的Cobra框架，解析命令行参数和子命令
  * 使用net/http包来调用 接口

### 审计系统: 优化日志架构，实现HA环境下的跨实例日志同步、持久化存储和高效查询。

* 使用shell脚本
  * rsync -avz --delete /var/log/audit.log nodeB:/var/log/
* crontab -e
  */1 * * * * /bin/bash /opt/sync_logs.sh
* 监听日志变更并实时同步

```shell
inotifywait -m /var/log/audit.log -e modify |
while read path action file; do
    rsync -avz --delete /var/log/audit.log nodeB:/var/log/
done
```



