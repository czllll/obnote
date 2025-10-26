
| domain      | layer | type  |
| ----------- | ----- | ----- |
| #middleware | #mq   | #fact |

* 基本概念
* 常见问题
	* 消息积压
	* 事务消息
	* kafka 分区 Leader 选举过程
	* kafka Controller 选举过程


## 基本概念
### Topic
Topic是一个逻辑概念
* 生产者发布的消息是需要指定一个Topic的
* 消费者是从它所指定订阅的 “主题”Topic 中去拉取的
* 一个主题Topic被分成多个Partition分区
### Patition
![image.png](https://img.dirtsai.work/astro-blog/2025/09/b5de0e64374d6466b98377475941898f.png)
- Partition分区 是最小的存储单元，掌握着一个Topic的部分数据。
- 每个 Partition 分区都是一个单独的 log 文件，每条记录都以追加的形式写入。
- 一个 Topic 的所有 Partition是分布在多个不同的Broker中的，目的是提高容错率、提高消息的消费能力，所有 Partition 的数据的并集就是所有数据
- 一个 Partition 会生成多个副本Replica（replication.factor = 3），并且把它们分散存储在不同的 Broker 中
##### 选举过程

### OffSet
在 kafka 中一共有两类 offset，一类是消息 offset，一类是消费 offset
#### 消息offset（LogStartOffset）
**定义**：Kafka 在Partition 内给每条消息分配的递增编号，从 0 开始，追加一条消息就 +1
**特点**：
	- 不可变，即使消息过期或被清理也不会重用。
	- 表示消息在分区 log 文件中的「位置」。
**存储位置**：和消息数据一起写在 **分区 log 文件** 中。
**作用**：用来唯一标识消息在分区中的顺序位置
#### 消费offset
**定义**：消费者组（Consumer Group）维护的消费进度标记，表示自己「已经消费到哪条消息」
**含义**：指向「下一个要消费的消息」。例如：提交 offset=5 → 表示 0~4 已经处理，下次从 5 开始拉取。
**作用**：用来记录消费者组的消费进度，实现故障恢复、重平衡等功能。
**存储位置**：
    - Kafka 0.9.0 之前：存储在 Zookeeper。 
    - Kafka 0.9.0 之后：存储在 Kafka 内置主题 `__consumer_offsets`（默认 50 分区，可配置）
##### 消费 Offset 的提交
- **自动提交**：
    - `enable.auto.commit=true`（默认）。
    - Kafka 客户端会周期性（由 `auto.commit.interval.ms` 控制）把当前消费进度写入 `__consumer_offsets`。
- **手动提交**：
    - `enable.auto.commit=false`。
    - 程序需显式调用 `commitSync` 或 `commitAsync` 提交进度。
    - 优点：能精确控制「什么时候确认已消费」，适合严格的业务场景。
- **风险点**：提交失败或延迟 → 可能导致重复消费或少量消息丢失，需要应用层设计幂等或补偿机制。

##### 消费Offset的重置
- **手动重置**：
    - 调用 `seek(offset)`：从指定位置重新消费。
    - 调用 `seekToBeginning()`：从最早的消息开始。
    - 调用 `seekToEnd()`：跳到最新位置，只消费新写入的消息。
- **自动重置**（由 `auto.offset.reset` 参数控制）：
    - `earliest`：没有已提交 offset 时，从最早的消息开始。
    - `latest`：没有已提交 offset 时，从最新消息开始。
    - `none`：如果没有 offset，直接抛异常。

### 消费者组
多个消费者，只要指定了相同的 group_id ，即属于同一个消费者组，同一个消费者组内的消费者可以共同消费一个Topic中的数据。PS不同消费者组也可以共同消费同一个Topic中的数据。
##### 分配策略
*消费者是怎么分配同一个 topic 下的 Partition？*
- **RangeAssignor**（默认）
    - 按照 Partition 的序号区间平均分配。
    - 例：有 6 个 Partition，2 个 Consumer → C1 拿 P0~~P2，C2 拿 P3~~P5。
    - 特点：如果 Topic 多，可能分配不均衡。
- **RoundRobinAssignor**
    - 按轮询分配。
    - 例：6 个 Partition，2 个 Consumer → C1 拿 P0、P2、P4，C2 拿 P1、P3、P5。
    - 特点：更均衡，尤其是多 Topic 时。
- **StickyAssignor**
    - 在保证尽量均衡的前提下，尽量保持和上一次分配结果一致，减少 Rebalance 带来的数据重复消费。
    - Kafka 2.4+ 默认用它代替 Range。
- **CooperativeStickyAssignor**（Kafka 2.4+ 推荐）
    - 比 Sticky 更优雅，支持**渐进式 Rebalance**，不需要一次性把所有分配推翻，只迁移必要的 Partition。
### AR 、ISR、OSR
* 分区中的所有副本统称为 AR（Assigned Replicas）
* 所有与 leader 副本保持一定程度同步的副本（包括 leader 副本在内）组成 ISR（In-Sync Replicas）
* 与leader副本同步滞后过多的副本（不包括leader副本）组成OSR（Out-of-Sync Replicas）
* 即有， AR=ISR+OSR，在正常情况下 OSR 应为 0
* *谁来维护*？
	* leader副本负责维护和跟踪ISR集合中所有follower副本的滞后状态
		* 当follower副本落后太多或失效时，leader副本会把它从ISR集合中剔除
		* 如果OSR集合中有follower副本“追上”了leader副本，那么leader副本会把它从OSR集合转移至ISR集合。
### HW和LEO
* HW（High Watermark），标识一个特定的消息偏移量（offset），消费者只能拉取到这个 offset 之前的消息 *（HW 是每个分区维护的？）*
* LEO（Log End Offset），标识当前日志文件中下一条待写入消息的offset
* 分区 ISR 集合中每个副本都会维护自身的 LEO，ISR 集合中最小的LEO即为分区的 HW


## 参数配置
### 服务端参数配置
* 放置在$KAFKA_HOME/config/server.properties文件中
* zookeeper.connect zk 集群地址
* listeners broker 监听客户端链接的地址列表
* broker.id kafka 集群 broker 唯一标识，默认-1
* log.dir 日志文件目录
* message.max.bytes mbroker 接收消息最大值 默认 1000012 约 976.6KB，超过会报异常



## 生产者
* 生产者通过`send()`方法发往 broker 的过程中，有可能需要经过拦截器，序列化器（必需）和分区器（指定分区字段则不需要）等一系列作用。

### 分区器
* kafka默认分区器实现`org.apache.kafka.clients.producer.Partitioner`接口，该接口定义了两个方法
	* `int partition(String topic, Object key, args...)` 计算分区号
		* 一般用 key 做 hash，key 为 null，消息将会轮询方式发送给可用分区
	* `void close()` 关闭分区器时用来回收资源
	* 可自行实现`Partitioner`接口实现自定义分区器

### 拦截器Interceptor
* 有生产者拦截器和消费者拦截器
* 实现`org.apache.kafka.clients.producer.ProducerInterceptor`接口以实现自定义拦截器
	* `onSend()`方法先于序列化器和分区器执行
	* 在其消息被应答/发送失败时调用生产者拦截器的`onAcknowledgement()`方法，优于用户设定的 Callback 之前执行，该方法运行在 Producer 的 IO 线程中，因此越简单越好
* 可以指定多个拦截器以形成拦截链

### 生产者架构
生产者客户端由两个线程协调运行：主线程，Sender 线程。
* 主线程线：
	* 由 KafkaProducer 创建消息，然后通过可能的拦截器，序列化器和分区器的作用后缓存到消息累加器(RecordAccumulator)
* Sender线
	* 负责从 RecordAccumulator 中获取消息并将其发送到 kafka
#### RecordAccumulator
##### 作用
 * 缓存消息支持 Sender 线程可以批量发送，减少网络传输的资源消耗以提升性能
 * 生产者发送消息的速度超过发送到服务器的速度，会导致生产者空间不足，KafkaProducer的 send方法要么被阻塞要么抛出异常
##### 结构
* 主线程发过来的消息都会被追加到 RecordAccumulator 的某个双端队列 *Deque*（每个分区都有一个 Deque） 中，队列中的内容是 ProducerBatch，即`Deque<ProducerBatch>`
* 内部有一块 *BufferPool*，主要用来实现`java.io.ByteBuffer`的复用，以实现缓存的高效利用，只会针对特定大小的 ByteBuffer 进行管理。
#### InFlightRequests
* 在 Sender 线程发往 Kafka 之前会保存到 InFlightRequests 中
* 作用： 缓存已经发出去但还没有收到相应的请求。
* `leastLoadedNode`,所有 Node 中负载最小的那一个就是通过缓存的数量确定的

### 元数据及其更新
* 元数据指 Kafka 集群的元数据，记录了
	* 集群中有哪些主题
	* 这些主题有哪些分区
	* 每个分区的 leader 分贝分配在哪个节点上
	* follower 副本分配在哪些节点上
	* 哪些副本在 AR、ISR 等级和中
	* 集群中有哪些节点
	* 控制器节点是哪一个
* client 中没有需要使用的元数据信息时，会先挑选书 leastLoadedNode，然后像这个 Node 发送
  （Sender 发送）MetadataRequest 请求来获取具体的元数据信息。在 Sender 线程更新后，主线程也需要读取

### 生产者参数
必填的三个
* bootstrap.servers 即 broker 的地址
* key.serializer/ value.serializer两个序列化器
其他重要参数
* acks
	* 指定分区中必须要由多少个副本收到这条消息，之后生产者才会认为这条消息是成功写入的
	* acks=1(默认值)
		* 问题：消息写入leader 副本并返回成功相应给生产者，且在被其他 follower 副本拉取之前的 leader 副本崩溃，消息还是会丢失，因为新选举的 leader 副本中并没有这条对应消息
	* acks=0，生产者发送消息后不需要等待任何服务端的响应。
		* 问题：生产者无从得知 msg 的到达情况。但是可以达到最大吞吐量
	* acks=-1 或 acks=all，等待 ISR 中所有副本写入成功后才能收到来自服务端的成功响应
		* 问题：不一定完全可靠，ISR 可能只有 leader 副本就得退化成 acks=1 了
* max.request.size
	* 限制客户端能发送消息的最大值，默认 1MB


## 消费者
* app 可以通过 KafkaConsumer 订阅主题，并从订阅的主题中拉取消息
### 消费者组（逻辑概念）
* 每个消费者都有一个对应的消费组，消息发布到主题后，只会被投递给订阅它的每个消费组中的一个消费者。
* 每一个分区只能被一个消费组中的一个消费者所消费
* 消费者消费组的模型使得 kafka 支持p2p 和发布订阅两种消息投递模式
	* 所有消费者都隶属于同一个消费组，相当于 p2p
	* 所有消费者属于不同的消费组，相当于 pub/sub

### 消费者参数
四个必填
* bootstrap.servers
* group.id
* key.deserializer
* value.deserializer
非必填
* auto.commit.commit 参数为 true 
	* 定期默认提交，在poll()方法里完成，每次真正向服务端发起拉取请求之前会检查是否可进行位移提交，若可以则提交上一次的轮询位移
* auto.commit.interval.ms 默认 5 秒

### 消费者客户端方法
* subscribe()订阅主题
* assign()订阅分区

### 消息消费
消息消费一般有两种模式：推模式，拉模式
Kafka 中消费基于拉模式，不断轮训 poll()方法，poll方法返回 ConsumerRecords，使用迭代器迭代
ConsumerRecords 的方法如下：
* `record(TopicPartition partition)` 只取某个特定分区的消息列表，可以更细粒度的提交 offset

### 提交Offset
* Offset 在 Kafka 中涉及两处概念
	* 一处是分区存储层面，在 Kafka 分区中，每一条群消息都有唯一的 offset，表示消息在分区中对应的位置
	* 另一处是消费者消费时，用 offset 标识消费到分区中某个消息所在的位置。Kafka 将该 offset 存储起来到 Kafka 内部的主题_consumer_offsets_中，该动作称为提交，消费者在消费完消息后需要执行 offset的提交。如消费了 x 信息，提交 x+1 表示下一次需要消费的位置(即 position)
* KafkaConsumer 类提供了 position(TopicPartition)和 committed(TopicPartition)来获取
	* position 是消费者在特定 TopicPartition 上消费的 offset，消费者短的信息
	* committed 是 Kafka 中记录的消费者在特定 TopicPartition 上已提交的 offset
	* 并不总是一样，即消费者消费未提交就有可能 commited 落后于 position
* 自动提交无法做到精确的 offset 管理
* 手动提交
	* 同步提交
		* 等 Kafka 确认该 offset 已经成功提交才继续消费下一条信息
	* 异步提交
		* 不等待 Kafka 确认

### 控制或关闭消费
```java
// 暂停某些分区在拉取操作时返回给客户端
public void pause(Collection<TopicPartition> partitions)
// 恢复某些分区返回给客户端数据
public void resume(Collection<TopicPartition> partitions)

```

### 指定位移消费
* 由于我们对 offset 进行了持久化（在 Kafka 内置主题 `__consumer_offsets`），因此消费者在关闭，崩溃或者 rebalance 时，接替消费者能够根据存储的消费offset继续进行消费
* 消费者客户端参数：`auto.offset.reset:latest/earliest/none`
	* 当查不到消费offset 的时候，从哪里开始消费
	* latest:从最后开始消费
	* earliest：从最开始
	* none：抛异常
* 使用 `public void seek(TopicPartition partition, long offset)`重置消费者位置

### Rebalance
* **定义**：指分区的所属权从一个消费者转移到另一个消费者的行为
* **限制**：在 rebalance 期间，消费组内的消费者无法读取消息；消费者被重新分配，若某消费者消费了消息但是没有提交 offset ，则可能发生重复消费
#### ConsumerRebalanceListener
* 一个接口，两个方法，用于设定发生rebalance 前后的一些动作
```java
// 在消费者停止读取消息，rebalance 之前被调用，处理消费位移的提交，partitions标识 rebalance 之前所分配到的分区
void onPartitionsRevoked(Collection＜TopicPartition＞partitions)
// 在重新分配分区后和消费者开始读取消费前调用， partitions标识 rebalance 之后所分配到的分区
void onPartitionsAssigned(Collection＜TopicPartition＞partitions)
```


### 消费者拦截器
* 自定义实现接口`org.apache.kafka.clients.consumer.ConsumerInterceptor`
```java
	// poll 方法返回之前调用
	public ConsumerRecords＜K,V＞onConsume(ConsumerRecords＜K,V＞records);
	// 提交完消费位移后
	public void onCommit(Map＜TopicPartition,OffsetAndMetadata＞offsets);
	// 关闭拦截器并释放资源
	public void close().
```

### 线程安全
* KafkaProducer是线程安全，但是KafkaConsumer非线程安全
`KafkaConsumer.acquire()`用来检测当前是否只有一个线程在操作，每个KafkaConsumer的 public 方法都会先调用这个 acquire，原理就是计数
* KafkaConsumer多线程实现方式
	* 线程封闭：为每一个下次能实例化一个KafkaConsumer对象
	* 多个消费线程同时消费同一个分区
	* 处理消息模块改成多线程的实现方式
* 问题：为什么KafkaProducer是线程安全，但是KafkaConsumer非线程安全？为什么生产者不能起多线程？？
	* KafkaProducer的 send 方法通常是异步的，发到缓冲区 batch 再发送
	* KafkaConsumer需要维护跟踪每个分区的偏移量，多个线程同时操作同一个消费者实例，可能导致对相同分区的并发消费
		* 问题：1. 偏移量更新不一致；分区顺序性不能保证；消费者分区状态不能保证
	* 

### 消费者参数
```yml
	fetch.min.bytes=1 #Consumer一次 poll 能拉去最小的数据量(1代表 1B)
	fetch.max.bytes=52428800 #最大，默认50MB
	
```



## 消息积压
### 消费者端优化
1. 消费者**消费速度太慢**，导致消息积压，可以优化消费者代码逻辑
2. 当消费者数量少于partition分区数量时，可以**增加消费者数量**（但不能超过partition的数量）
3. 当手动提交offset时，**检查是否消费者未正确提交 offset**，导致消费停滞
### 生产者端优化
1. **控制生产速度**：例如在生产者端设置限流机制，避免消息生产速度过快
### Kafka集群优化
1. **调整分区数量**：根据消息生产和消费速度，合理调整主题的分区数量。如果消息堆积是由于分区数过少导致，可增加分区数。例如，将一个原本只有2个分区的主题，根据业务量增加到10个分区，以**提高并行处理能力**。但分区数过多也会增加管理开销，需谨慎评估。


## 主题与分区
### 主题
* 可以通过`kafka-topics.sh`脚本或者`KafkaAdminClient`客户端发送请求来对主题进行操作
* 脚本实际上调用`kafka.admin.TopicCommand`类
* 
### 主题管理
#### 创建主题
* `auto.create.topics.enable = true`情况下默认创建主题，非预期行为
* 推荐通过`kafka-topics.sh`脚本创建主题
	* `[root@node1 kafka_2.11-2.0.0]# bin/kafka-topics.sh --zookeeper localhost:2181/kafka --create --topic topic-create --partitions 4 --replication-factor 2`
	* 4 个参数
		* zookeeper zk 链接地址
		* partitions 分区数
		* replication-factor 副本因子
		* topic 主题
* 注意事项：
	* 主题明明不推荐双下划线开头，一般为内部主体
	* kafka 内部会根据主题名称明明metric 会把"."改成"\_"
##### 创建主题时分区分配算法
```scala
	private def assignReplicasToBrokersRackUnaware(
	  nPartitions: Int,            // 分区数
	  replicationFactor: Int,      // 副本因子
	  brokerList: Seq[Int],        // broker 列表
	  fixedStartIndex: Int,        // 起始索引，默认为 -1
	  startPartitionId: Int       // 起始分区 ID，默认为 -1
	): Map[Int, Seq[Int]] = {     // 返回分配结果
	  val ret = mutable.Map[Int, Seq[Int]]()  // 保存分配结果的集合
	  val brokerArray = brokerList.toArray    // broker 的数组
	  // 如果 fixedStartIndex 小于 0，随机从 broker 列表中选择一个作为分区的起始位置
	  val startIndex = if (fixedStartIndex >= 0) fixedStartIndex else rand.nextInt(brokerArray.length)
	
	  var currentPartitionId = math.max(0, startPartitionId)  // 设置起始分区 ID，确保 >= 0
	  var nextReplicaShift = if (fixedStartIndex >= 0) fixedStartIndex else rand.nextInt(brokerArray.length)
	
	  // 遍历所有分区，将每个分区的副本分配到不同的 broker 上
	  for (_ <- 0 until nPartitions) {
	    // 如果当前分区 ID 对 broker 数量取余为 0，进行副本迁移
	    if (currentPartitionId % brokerArray.length == 0)
	      nextReplicaShift += 1
	
	    // 计算该分区副本的起始索引
	    val firstReplicaIndex = (currentPartitionId + startIndex) % brokerArray.length
	    var replicaBuffer = mutable.ArrayBuffer(brokerArray(firstReplicaIndex))
	
	    // 为该分区分配剩余的副本
	    for (j <- 0 until replicationFactor - 1) {
	      replicaBuffer += brokerArray((firstReplicaIndex + j + nextReplicaShift) % brokerArray.length)
	    }
	
	    // 保存分配的副本信息
	    ret.put(currentPartitionId, replicaBuffer)
	    currentPartitionId += 1
	  }
	  ret
	}

```

* 即在哪个 broker 中创建哪些分区的副本
* 若指定了 replica-assignment参数，就按照指定方法创建
* 未使用该参数就按照内部逻辑
	* 未配置 broker.rack参数或者使用 disable-rack-aware参数则采用 *未指定rack信息的分配策略*
		* 
#### 查看主题
* `-list` 查看当前所有可用主题
* `-describe`查看单个主体信息
#### 修改主题
* 修改分区个数（只支持增加，为什么（因为减少分区要处理删除分区中的消息）），修改配置
* `-alter`
#### 配置管理
* `--entity-type`
#### 删除主题
* `--delete`

### 使用 KafkaAdminClient
```java

// 创建主题
CreateTopicsResult createTopics(Collection<NewTopic> newTopics);

// 删除主题
DeleteTopicsResult deleteTopics(Collection<String> topics);

// 列出所有可用的主题
ListTopicsResult listTopics();

// 查看主题的信息
DescribeTopicsResult describeTopics(Collection<String> topicNames);

// 查询配置信息
DescribeConfigsResult describeConfigs(Collection<ConfigResource> resources);

// 修改配置信息
AlterConfigsResult alterConfigs(Map<ConfigResource, Config> configs);

// 增加分区
CreatePartitionsResult createPartitions(Map<String, NewPartitions> newPartitions);

```


### 分区
#### 优先副本选举
**分区特性**
* 分区使用多副本机制提升可靠性
	* leader 副本对外提供读写服务
	* follower 副本只负责在内部进行消息同步
	* 一个 broker 中最多只能有它的一个副本
**什么是优先副本**
* 指某分区在 AR 集合列表中的第一个副本，理想情况下优先副本就是该分区的 leader 副本，也可称为 preferred leader
**优先副本选举**
* 通过一定方式促使优先副本选举为 leader 副本，促进集群负载均衡
* 
#### 分区
#### 复制限流，修改副本因子
