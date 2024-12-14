* 类oneapi项目，参考yupiapi项目实现
* 前端：
	* nextjs
	* shadcn
* 后端
	* springboot
* 服务层
	* springboot


问题：
	1. 支持一个账号在多个地点登录？同时登录以后，有个人有一个账号对密码做了修改，那其他的账号会不会被踢掉呢？
	2. 你现在上线和下线接口的时候，就比如说刚刚你不是你客户端可以在多个地方登录。那这个时候就比如说A登录了也B也登录了，对吧？然后假设A对一个接口进行上线，然后B同时对这个接口进行下线，那你怎么样保证他们的操作是有序的？
	3. Dubbo用在了哪里
	4. 用什么来去根据什么来划分这些模块的？
	5. 你说你可以直接集成这个客户端SDK，那我API网关怎么统计你对这个客户端的调用情况呢？
	6. 如果客户端安装自己的SDK的话，你客户端SDK更新了以后，你怎么怎么做一个适配或者说发布
	7. 可能解决不了适配问题。比如说我有一个接口，我原来只要传三个参数，我老的业务只支持三个参数。那后面业务发生变动了，我需要传四个参数，那四个都是必传的那我新版的SDK发布了，我怎么让老的用户去使用我的新的SDK，或者说我怎么去把这个老APP适配掉？
	8. 相关项目
	9. 收获
	10. 用一个正常的AK使劲访问你，你有下线的操作吗
	11. 请求频率限制
	12. API虚拟认证算法
	13. 分布式的项目，你怎么部署的？比如说我有五个子模块，它们有相互依赖的关系的话
	14. 调用次数是怎么统计
	15. 涉及了API的签名认证的一个算法，给用户分配分分配了独立的那个AK用于健全，对吧？对，那那我想问一下你这个里面的话有没有防篡改相关的假设？（没看懂问题）
	16. 签名认证算法参考？
	17. 抽象出了一些公共模块。那您具体抽象了哪些公共模块？然后抽象的一个原则是什么样子的？中间有没有用到什么样的一些设计模式？
	18. RPC框架实现子系统间的高性能接口调用，然后的话您的接口调用大概有多少的QPS
	19. 网关作用


# 过程中的问题
* Restcontroller
* jakarta java14？
* spring配置是如何起作用的
* SpringMVC Servlet -- dispaturerServelet
* * 因为 `DispatcherServlet` 是整个 Spring MVC 的入口。
* * 项目结构，DTO，VO啥的都是啥啊
* 索引是干几把啥的？
## 注解
* @tablelogic

* `@Serial` 注解是在 Java 14 引入的

1. `@Data` (来自 Lombok)

- 自动生成所有字段的 getter 和 setter 方法
- 自动生成 toString() 方法
- 自动生成 equals() 和 hashCode() 方法
- 自动生成无参构造方法
- 相当于同时使用了 @Getter、@Setter、@ToString、@EqualsAndHashCode 和 @RequiredArgsConstructor

2. `@TableName(value = "user")` (来自 MyBatis-Plus)

- 指定实体类对应的数据库表名
- value = "user" 表示这个实体类对应数据库中的 "user" 表
- 如果类名和表名一致可以省略，不一致则必须指定

3. `@TableId(type = IdType.AUTO)` (来自 MyBatis-Plus)

- 指定主键生成策略
- type = IdType.AUTO 表示数据库自增主键
- 常用的 IdType 类型有：
    - AUTO: 数据库自增
    - NONE: 无状态，该类型为未设置主键类型
    - INPUT: 用户手动输入
    - ASSIGN_ID: 雪花算法生成id (默认实现)
    - ASSIGN_UUID: UUID生成

spring MVC controller 使用@valid 注解 验证 validation引入的注解 



----------------
@Resource 是 Java 的依赖注入注解，用于自动装配（注入）其他组件。简单说就是自动帮你把需要的对象注入进来，不用手动 new。

来看实际例子：

1. 基础使用：

java

Copy

```
@Service
public class UserServiceImpl {
    // 注入 UserMapper
    @Resource
    private UserMapper userMapper;
    
    public User getUser(Long id) {
        // 直接使用userMapper，不用手动创建
        return userMapper.selectById(id);
    }
}
```

不使用 @Resource 的话你得这样写：

java

Copy

```
@Service
public class UserServiceImpl {
    // 手动创建，很麻烦，而且管理困难
    private UserMapper userMapper = new UserMapper();
}
```

2. @Resource 和 @Autowired 的区别：

java

Copy

```
// @Resource 默认按名字匹配，找不到再按类型匹配
@Resource
private UserMapper userMapper;

// @Autowired 默认按类型匹配
@Autowired
private UserMapper userMapper;

// @Resource 可以指定名字
@Resource(name = "userMapperImpl")
private UserMapper userMapper;
```

3. 常见使用场景：

java

Copy

```
@Service
public class UserServiceImpl {
    // 注入Mapper
    @Resource
    private UserMapper userMapper;
    
    // 注入其他Service
    @Resource
    private OrderService orderService;
    
    // 注入配置
    @Resource
    private Configuration config;
    
    // 注入工具类
    @Resource
    private RedisUtil redisUtil;
}
```

4. 使用建议：

java

Copy

```
@Service
public class UserServiceImpl {
    // 1. 建议：用在属性上
    @Resource
    private UserMapper userMapper;
    
    // 2. 不建议：用在setter方法上
    @Resource
    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }
    
    // 3. 建议：如果有多个同类型的bean，指定name
    @Resource(name = "masterDataSource")
    private DataSource masterDataSource;
    
    @Resource(name = "slaveDataSource")
    private DataSource slaveDataSource;
}
```

主要作用：

1. d自动注入依赖对象
2. 管理对象的生命周期
3. 降低代码耦合度
4. 方便测试和维护

--------------------


## mybatis
这三个是MyBatis常用的注解，我来详细解释：

1. `@Mapper`

java

Copy

```
@Mapper
public interface UserMapper {
    // 标记这是一个Mybatis的Mapper接口
}
```

- 作用：标识这是一个MyBatis的Mapper接口
- 让Spring可以扫描到这个接口并创建实现类
- 可以用`@MapperScan("包路径")`代替，避免每个Mapper都加`@Mapper`

2. `@Param`

java

Copy

```
public interface UserMapper {
    // 单参数
    User findById(@Param("id") Long id);
    
    // 多参数
    List<User> findByNameAndAge(
        @Param("name") String name, 
        @Param("age") Integer age
    );
}
```

- 作用：给参数命名，在SQL中引用这个名字
- 当有多个参数时，强烈建议使用@Param
- XML中使用：#{name}, #{age}来引用参数

3. `@Select`

java

Copy

```
@Mapper
public interface UserMapper {
    @Select("SELECT * FROM user WHERE id = #{id}")
    User findById(@Param("id") Long id);
    
    @Select("SELECT * FROM user " +
           "WHERE name = #{name} " +
           "AND age = #{age}")
    List<User> findByNameAndAge(
        @Param("name") String name, 
        @Param("age") Integer age
    );
}
```

- 作用：直接在接口方法上编写SQL语句
- 适合简单的SQL查询
- 复杂SQL建议使用XML配置

## java
### 泛型
* 编译报错和运行时报错
* 自动推断类型的泛型方法
泛型的主要作用是实现"类型安全"和"代码复用"。我用具体例子说明：

1. 不用泛型的情况：


java

Copy

```
// 1. 不用泛型的List
List list = new ArrayList();
list.add("字符串");
list.add(123);
list.add(new User());  // 可以添加任何类型，不安全！

// 取值时需要强制类型转换，可能报错
String str = (String)list.get(0);  // 正常
String str2 = (String)list.get(1); // 运行时报错！因为存的是数字

// 2. 不用泛型的方法
public Object findById(Object id) {
    // 需要强制类型转换，容易出错
    return new Object();
}
```

2. 使用泛型的情况：

java

Copy

```
// 1. 使用泛型的List
List<String> list = new ArrayList<>();
list.add("字符串");    // 正常
list.add(123);        // 编译错误！类型检查不通过
list.add(new User()); // 编译错误！类型检查不通过

// 取值时不需要强制转换，类型安全
String str = list.get(0);  // 直接获取String类型

// 2. 泛型方法
public <T> T findById(Long id) {
    return mapper.selectById(id);
}

// 3. 泛型类
public class Result<T> {
    private Integer code;
    private String msg;
    private T data;  // 数据类型由使用时指定
    
    // 可以返回任何类型的结果
    Result<User> userResult = new Result<>();
    Result<List<Order>> orderListResult = new Result<>();
}
```

3. MyBatis-Plus中的泛型使用：

java

Copy

```
// 1. BaseMapper使用泛型
public interface UserMapper extends BaseMapper<User> {
    // 指定了实体类型是User后：
    // insert方法只接受User类型参数
    // select方法返回值自动为User类型
    // 不会插入错误类型的数据
}

// 2. Service使用泛型
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> {
    
    public void example() {
        // 保存时只能保存User对象
        save(new User());        // 正确
        save(new Order());       // 编译错误！
        
        // 查询时直接返回User对象
        User user = getById(1L); // 不需要强制转换
        
        // 批量操作时只能操作User集合
        List<User> users = list();
        saveBatch(users);        // 正确
    }
}
```

4. 常见泛型场景：

java

Copy

```
// 集合
List<User> userList;              // 用户列表
Map<String, Object> paramMap;     // 参数映射
Set<Long> idSet;                  // ID集合

// 分页
Page<User> userPage;              // 用户分页
IPage<OrderVO> orderVOPage;       // 订单VO分页

// 响应对象
Result<User> userResult;          // 单个用户响应
Result<List<User>> userListResult;// 用户列表响应

// 查询条件
LambdaQueryWrapper<User> wrapper; // User表查询条件
```

泛型的好处：

1. 类型安全，编译时就能发现类型错误
2. 避免强制类型转换
3. 代码复用，同一个类可以处理不同类型
4. 提升代码可读性
5. IDE可以提供更好的代码提示

实际开发建议：

1. 优先使用泛型
2. 明确指定类型参数
3. 避免使用原生类型(raw type)
4. 合理使用泛型通配符
5. 注意泛型类型擦除的影响


### 表达式
getOne(this.lambdaQuery().eq(Model::getModelName, modelName)).getRealApiKey();

## 前端
* 全局请求的响应拦截器和请求拦截器
* 



# todo
* 发布接口
	* 校验接口是否存在
	* 判断接口是否可以调用
	* 修改状态字段为1
* 下线接口
	* 校验接口是否存在
	* 修改状态字段为0
* 两个key集成在user表里

* 限流
	* 令牌桶
	* 漏桶
	* redis limit handler

* common 提取了什么 service 和 model




Copy

```
- 动态模型选择
  • 根据请求内容智能选择最适合的模型
  • 自动在同类模型间做成本优化
  • 支持模型组合调用(先用便宜模型,不满意再用贵的)

- 高级负载均衡
  • 根据模型响应时间动态调整权重
  • 支持就近地域路由
  • 多渠道key自动切换
```

2. 企业级安全特性

Copy

```
- 数据安全
  • 敏感信息脱敏
  • 关键词过滤
  • 内容审计日志

- 访问控制
  • 细粒度API权限
  • IP白名单
  • 调用频率限制
  • 动态黑名单
```

3. 高级计费功能

Copy

```
- 弹性计费
  • 阶梯价格
  • 包量包时计费
  • 预付费/后付费
  • 多币种支持

- 额度管理
  • 预警规则
  • 透支额度
  • 临时额度
  • 额度自动补充
```

4. 业务分析能力

Copy

```
- 调用分析
  • 调用链详情
  • 错误分析
  • 性能分析
  • 成本分析

- 数据大盘
  • 实时监控
  • 趋势分析
  • 用户画像
  • ROI分析
```

5. 高级模型管理

Copy

```
- 模型编排
  • 串行调用
  • 并行调用
  • 条件调用
  • 结果合并

- 模型优化
  • 参数自动调优
  • 响应缓存
  • 结果重用
  • 批量处理
```

6. 开发者友好

Copy

```
- SDK支持
  • 多语言SDK
  • 示例代码
  • Playground

- 接口增强
  • WebSocket支持
  • 流式处理
  • 批量接口
  • 异步回调
```

7. 运维能力

Copy

```
- 监控告警
  • 多维度监控
  • 智能告警
  • 故障自愈

- 运营分析
  • 用户行为分析
  • 使用率分析
  • 转化率分析
  • 营收分析
```

8. 扩展性设计

Copy

```
- 插件机制
  • 自定义认证
  • 自定义限流
  • 自定义计费
  • 中间件扩展

- 开放能力
  • Webhook
  • 事件订阅
  • 自定义模型接入
  • 三方系统集成
```

9. AI增强特性

Copy

```
- 智能运维
  • 异常检测
  • 容量预测
  • 成本优化

- 智能分析
  • 用户意图识别
  • 场景匹配
  • 效果评估
  • 质量打分
```

10. 高可用设计

Copy

```
- 服务容错
  • 多级降级
  • 服务隔离
  • 限流熔断
  • 自动重试

- 数据可靠
  • 分布式事务
  • 数据一致性
  • 灾备切换
  • 数据热备
```


## 步骤
第一阶段：基础框架搭建

markdown

Copy

```
1. 技术选型
   - 微服务框架: Spring Cloud Alibaba
   - 服务注册发现: Nacos
   - 数据库: MySQL + Mybatis-Plus
   - 缓存: Redis
   - 消息队列: RocketMQ
   - 网关: Spring Cloud Gateway
2. 项目结构
root-project
├── common
│   ├── core
│   ├── redis
│   ├── database
│   └── mq
├── gateway-service
├── user-service
├── key-service
├── billing-service
└── model-proxy-service

3. 核心功能实现
   - 统一认证
   - 服务间调用
   - 基础CRUD
   - 简单计费
```

第二阶段：消息事件体系

reasonml

Copy

````
1. RocketMQ集成
   - 生产者配置
   - 消费者组
   - 消息重试
   - 事务消息

2. 典型消息流
用户注册流程：
User-Service ---> [注册成功事件] ---> MQ
                                    ├── Billing-Service(创建账户)
                                    └── Key-Service(初始化配置)

调用计费流程：
Model-Proxy ---> [调用记录事件] ---> MQ
                                  ├── Billing-Service(扣费)
                                  └── Statistics-Service(统计)

3. 消息处理示例代码:
```java
// 生产者
@Service
public class UserEventProducer {
    @Resource
    private RocketMQTemplate rocketMQTemplate;
    
    public void sendUserRegisterEvent(UserRegisterDTO dto) {
        Message<UserRegisterDTO> message = MessageBuilder
            .withPayload(dto)
            .setHeader(KEYS, dto.getUserId())
            .build();
        rocketMQTemplate.send("user-register-topic", message);
    }
}

// 消费者
@Component
@RocketMQMessageListener(
    consumerGroup = "billing-service-group",
    topic = "user-register-topic"
)
public class UserRegisterConsumer implements RocketMQListener<UserRegisterDTO> {
    @Override
    public void onMessage(UserRegisterDTO dto) {
        // 处理用户注册后的账户初始化
    }
}
````

第三阶段：高级特性实现

markdown

Copy

```
1. 分布式事务
   - Seata集成
   - AT模式
   - SAGA模式

2. 限流熔断
   - Sentinel接入
   - 限流规则
   - 熔断降级

3. 调用链监控
   - SkyWalking集成
   - 链路追踪
   - 性能分析
```

第四阶段：运维体系

markdown

Copy

```
1. ELK日志体系
   - LogStash收集
   - ES存储
   - Kibana展示

2. 监控告警
   - Prometheus指标
   - Grafana仪表盘
   - 告警规则

3. 容器化部署
   - Dockerfile
   - Docker Compose
   - K8s编排
```

# spring
是的，添加扫描确实不太优雅。更好的方式是使用Spring Boot的自动配置机制：

1. **在common包中创建自动配置类**：

java

Copy

```
// common/src/main/java/work/dirtsai/common/config/CommonAutoConfiguration.java
@Configuration
public class CommonAutoConfiguration {
    @Bean
    public JwtUtils jwtUtils() {
        return new JwtUtils();
    }
}
```

2. **创建spring.factories文件**：

properties

Copy

```
# common/src/main/resources/META-INF/spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
work.dirtsai.common.config.CommonAutoConfiguration
```

现在不需要在主应用程序添加@ComponentScan了，Spring Boot会自动加载这些配置。

这种方式的优点：

1. 遵循Spring Boot的自动配置机制
2. 使用方不需要额外配置
3. 解耦合，更优雅
4. common包可以作为一个独立的starter

另外，如果需要条件装配：

java

Copy

```
@Configuration
@ConditionalOnClass(JwtUtils.class)  // 当类路径下有JwtUtils时才装配
public class CommonAutoConfiguration {
    @ConditionalOnMissingBean  // 当容器中没有该Bean时才创建
    @Bean
    public JwtUtils jwtUtils() {
        return new JwtUtils();
    }
}
```

这样的设计更符合Spring Boot的规范。


