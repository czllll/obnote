## Spring

### 说一下你对Spring的理解？

2. **IoC 容器（控制反转）**

   - 通过 IoC 机制由容器负责对象的创建与依赖注入，开发者无需手动创建和管理依赖关系
   - 本质是将对象的控制权从程序代码中转移到 Spring 容器中
   - 支持多种注入方式：构造器注入、Setter 注入、注解注入等

3. **AOP（面向切面编程）**

   - 用于提取系统中的横切关注点，例如日志、安全、事务等
   - 提高系统的模块化、解耦性和可维护性
   - 核心概念包括：
     - 切面（Aspect）
     - 通知（Advice）
     - 连接点（JoinPoint）
     - 切入点（Pointcut）
     - 织入（Weaving）
   - Spring AOP 基于 JDK 动态代理或 CGLIB 实现

4. **事务管理（Transaction Management）**

   - 提供统一的事务处理接口，支持编程式与声明式事务
   - 兼容多种事务管理器：DataSourceTransactionManager、JpaTransactionManager 等
   - 声明式事务通常使用 `@Transactional` 注解配置
   - 可实现事务传播行为、隔离级别、回滚规则等高级事务控制

5. **Spring MVC 框架**

   - 基于 Servlet 的 Web 框架，实现了 MVC 架构模式
   - 特点包括：
     - 支持请求分发、参数绑定、数据验证、异常处理等
     - 提供注解方式开发（如 `@Controller`、`@RequestMapping`）
     - 支持多种视图解析器：JSP、Thymeleaf、FreeMarker 等
     - 与 IoC 容器深度集成，实现统一的 Bean 管理



### spring的核心思想说说你的理解？

1. **Spring 的三大核心思想**

   Spring 的设计理念围绕三个核心思想展开：IOC（控制反转）、DI（依赖注入）、AOP（面向切面编程），它们共同构建了轻量级、高内聚、低耦合的企业级应用开发框架。

2. **IOC（Inversion of Control，控制反转）**

   - 解决的问题：
     - 对象创建与依赖管理的高度耦合问题
   - 实现手段：
     - 容器管理 Bean 的生命周期，由框架负责对象的创建与装配
   - 应用场景：
     - 动态替换数据库实现
     - 服务对象组装（如组合多个子服务为一个业务服务）

3. **DI（Dependency Injection，依赖注入）**

   - 解决的问题：
     - 对象之间的依赖关系硬编码，耦合度高、测试困难
   - 实现手段：
     - 使用构造器注入、Setter 注入或注解注入，将依赖对象注入目标类中
   - 应用场景：
     - 注入数据源、DAO 层对象
     - 服务层依赖注入 DAO 层，提高可测试性与解耦性

4. **AOP（Aspect Oriented Programming，面向切面编程）**

   - 解决的问题：
     - 横切逻辑（如日志、事务、安全）分散在业务逻辑中，导致代码混乱、重复
   - 实现手段：
     - 使用动态代理技术与切面配置，将横切逻辑抽离到统一位置管理
   - 应用场景：
     - 日志记录
     - 事务控制
     - 权限校验、统一异常处理等

5. **总结**

   - IOC、DI、AOP 是 Spring 的三大核心理念：
     - IOC 是指导思想，DI 是实现方式，AOP 是补充机制
   - 通过它们，Spring 实现了松耦合、模块化、可测试、可扩展的架构设计
   - 是构建 Java 企业级应用的重要基础框架

### Spring IoC和AOP介绍一下？

1. **IoC（控制反转）**
   - 概念：
     - 控制反转是一种创建和获取对象的技术思想，本质是将对象的创建控制权交给容器，而不是在代码中主动 `new` 对象。
   - 实现方式：
     - 依赖注入（DI）是 IoC 的具体实现形式，常用注入方式包括构造器注入、Setter 注入、注解注入等。
   - 优点：
     - 降低对象之间的耦合度，提升模块独立性和可测试性。
   - 应用示例：
     - 使用 IoC 容器管理 Service 和 DAO 层的依赖关系，由容器负责自动注入对象。
2. **AOP（面向切面编程）**
   - 概念：
     - AOP 用于将与业务无关的横切逻辑（如日志、事务、安全等）从核心业务逻辑中抽离出来，形成“切面”。
   - 作用：
     - 降低模块之间的耦合度，减少重复代码，提高可维护性。
   - 实现方式：
     - 基于动态代理实现，常用两种方式：
       - JDK 动态代理：用于代理实现了接口的类。
       - CGLIB 动态代理：用于代理未实现接口的类，通过生成子类方式实现。
   - 应用示例：
     - 在 Service 层统一处理日志记录、事务控制、权限校验等横切逻辑。
3. **IoC 与 AOP 的关系与结合使用**
   - IoC 负责对象创建和依赖关系管理，AOP 负责横切逻辑的统一处理。
   - 在实际开发中，二者常结合使用，实现良好的模块解耦与分层管理。
   - 典型用法：
     - 通过 IoC 管理业务 Bean 的生命周期与依赖注入
     - 通过 AOP 实现统一的日志记录、事务管理、异常处理等逻辑切面
4. **示例说明**
   - 通过 IoC 容器管理对象依赖关系
   - 通过 AOP 将横切关注点（如事务、日志）统一织入到业务逻辑中
   - 示例场景：
     - IoC 注入 DAO 到 Service，AOP 统一拦截 Service 中的方法，加入事务与日志功能
5. **总结**
   - IoC 和 AOP 是 Spring 框架的两大支柱：
     - IoC 解耦对象之间的依赖，推动“组件式开发”
     - AOP 解耦横切逻辑，提高系统的模块化和可维护性
   - 二者结合，可以实现清晰的系统分层结构与统一的逻辑管理机制

### Spring的aop介绍一下

1. **Spring AOP 简介**
   - AOP（面向切面编程）是 Spring 框架中的重要模块，用于实现核心业务逻辑之外的“横切关注点”的统一管理。
   - 它通过将日志、事务、安全等非业务逻辑从核心代码中剥离出来，实现逻辑解耦、职责分离和模块复用。
2. **AOP 中的两类功能**
   - 核心业务：增删改查、支付、下单、注册等主要逻辑
   - 周边功能：日志记录、事务处理、权限控制、异常处理等横切关注点
3. **AOP 的价值**
   - 把横切关注点从业务模块中抽离，封装为独立切面，实现模块解耦
   - 降低代码重复率，提高可维护性与扩展性
   - 统一管理安全、事务、日志等公共功能
4. **Spring AOP 的核心概念**
   - Aspect（切面）：
     - 是横切关注点的模块化封装单位，本质上是一个类
     - 通常包含多个通知（Advice）和切入点（Pointcut）
   - JoinPoint（连接点）：
     - 程序执行过程中的一个点，例如方法调用、异常抛出
     - Spring AOP 仅支持方法级别的连接点（Method Execution）
   - Advice（通知）：
     - 切面中的实际操作，定义切入逻辑
     - 类型包括：
       - 前置通知（Before）
       - 后置通知（After/AfterReturning）
       - 环绕通知（Around）
       - 异常通知（AfterThrowing）
   - Pointcut（切入点）：
     - 定义匹配哪些 JoinPoint 的规则，通常使用表达式指定方法路径或签名
   - Introduction（引入）：
     - 动态地为类添加新的属性或方法（接口），用于增强目标类的功能
   - Weaving（织入）：
     - 将切面应用到目标对象的过程
     - 时机分为：
       - 编译期（AspectJ）
       - 类加载期（类加载器增强）
       - 运行期（Spring AOP 采用运行期动态代理）
5. **Spring AOP 的实现原理**
   - 基于代理模式实现，分为两种方式：
     - JDK 动态代理：
       - 针对实现了接口的类生成代理对象
     - CGLIB 动态代理：
       - 针对没有实现接口的类，通过子类继承生成代理对象
6. **Spring AOP 与 AspectJ 的区别**
   - AspectJ 是一个完整的 AOP 框架，支持编译期和加载期织入
   - Spring AOP 是运行期 AOP 框架，专注于方法级代理，基于 Java 动态代理机制实现，适用于大多数企业级开发场景
7. **Target Object（目标对象）**
   - 被代理的对象称为目标对象，代理逻辑最终会调用目标对象的方法

### IOC和AOP是通过什么机制来实现的?

1. **Spring IoC 实现机制**

   IoC 容器的实现依赖于多种机制共同协作：

   - **反射机制**
     - Spring IoC 容器通过 Java 的反射机制动态加载类、创建对象实例、调用对象方法。
     - 支持在运行时检查类、属性、方法等信息，实现对象的灵活构建与管理。
   - **依赖注入（DI）**
     - 依赖注入是 IoC 的核心实现方式。
     - 容器负责组件之间依赖关系的维护，常见注入方式包括：
       - 构造器注入
       - Setter 方法注入
       - 注解注入（如 `@Autowired`, `@Value`）
   - **设计模式 - 工厂模式**
     - IoC 容器本质是一个大工厂，负责对象的创建、初始化、管理和销毁。
     - 通过工厂模式将对象实例化的职责从用户代码转移到容器。
   - **容器实现**
     - Spring 提供两种核心容器：
       - `BeanFactory`：基础容器，实现 IoC 的核心功能
       - `ApplicationContext`：`BeanFactory` 的扩展，提供事件发布、国际化、AOP 等高级企业级功能
     - 容器负责 Bean 的管理、依赖注入、生命周期管理等职责。

2. **Spring AOP 实现机制**

   Spring AOP 的实现基于 **动态代理技术**，不依赖编译期织入，而是运行时生成代理对象，从而实现方法增强。

   - **核心原理：动态代理**
     - 在运行时为目标对象生成代理类，对方法进行增强
     - 不修改源代码即可为现有逻辑添加功能，如日志、事务、安全控制等
   - **两种动态代理方式**
     - **基于 JDK 的动态代理**
       - 使用 `java.lang.reflect.Proxy` 和 `InvocationHandler` 实现
       - 要求目标类必须实现至少一个接口
       - 常用于接口式编程场景
     - **基于 CGLIB 的动态代理**
       - 使用 CGLIB（Code Generation Library）生成子类代理对象
       - 适用于目标类未实现接口的情况
       - 代理原理为“继承目标类，重写方法”
   - **自动代理选择**
     - Spring 会根据目标类是否实现接口自动选择 JDK Proxy 或 CGLIB 作为代理方式
     - 推荐编程时尽量使用接口，兼容性更好

### 怎么理解SpringIoc？

1. **什么是 IoC（Inversion of Control，控制反转）**
   - 是一种设计思想，将对象的创建与依赖关系的控制权从程序员转交给容器（如 Spring）。
   - 传统方式由程序主动创建对象，现在由 Spring 容器统一创建和注入依赖对象。
2. **传统方式与 Spring IoC 的对比**
   - 传统方式（无 IoC）：
     - 对象由程序主动创建和组合，耦合度高，测试困难。
     - 示例代码：
       - `Student student = new Student();`
       - `Score score = new Score();`
       - `student.setScore(score);`
   - Spring IoC 模式：
     - 对象由容器统一创建，依赖通过配置或注解注入。
     - 示例代码：
       - `ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");`
       - `Student student = context.getBean(Student.class);`
3. **IoC 控制的三个方面**
   - 创建对象：从程序中手动 new，变为由容器自动创建。
   - 初始化对象：从程序手动设置依赖，变为容器通过注入设置。
   - 销毁对象：从程序手动回收，变为容器统一生命周期管理。
4. **实现 IoC 的方式**
   - 依赖注入（DI）：
     - 构造器注入
     - Setter 注入
     - 注解注入（如 `@Autowired`）
   - 配置方式：
     - XML 配置
     - 注解配置
     - Java Config 配置类（如 `@Configuration` + `@Bean`）
5. **“控制反转” 的含义**
   - 控制：对象的创建、初始化、销毁等行为。
   - 反转：这些控制权从程序员转移给了 Spring 容器，程序从“主动依赖”变成了“被动注入”。
6. **IoC 带来的好处**
   - 降低类之间的耦合度
   - 提高代码的复用性、可测试性与可维护性
   - 便于实现模块化和解耦式开发

### 依赖倒置，依赖注入，控制反转分别是什么？

1. **控制反转（IoC，Inversion of Control）**
   - “控制”是指程序中对象创建与流程控制的主动权。
   - “反转”是指开发者不再手动控制整个程序流程，而是将控制权交给框架或容器。
   - 作用：让框架掌控应用程序的执行流程，开发者专注于业务逻辑。
2. **依赖注入（DI，Dependency Injection）**
   - 是实现控制反转的一种具体方式，是 IoC 的落地技术手段。
   - 不再在类内部通过 `new` 创建依赖对象，而是通过构造方法、Setter 方法或注解等形式，从外部“注入”依赖。
   - 本质上是将对象之间的依赖关系交由容器管理和注入。
3. **依赖倒置原则（DIP，Dependency Inversion Principle）**
   - 是一种面向架构设计的思想，属于 SOLID 原则之一。
   - 高层模块不应该依赖低层模块，二者都应依赖抽象。
   - 抽象不应该依赖细节，细节应该依赖抽象。
   - 强调程序应面向接口编程而非面向实现编程。
4. **三者之间的关系总结**
   - 控制反转是一种架构思想，强调“谁控制谁”。
   - 依赖注入是实现控制反转的方式，强调“如何注入依赖”。
   - 依赖倒置是一种设计原则，强调“模块间应依赖抽象而非具体实现”。
   - 三者结合构成了面向解耦、易测试、灵活扩展的架构核心。

### 依赖倒置，依赖注入，控制反转分别是什么？

 **什么是依赖注入（DI）**

- 在传统编程中，一个类需要依赖另一个类时，通常会在类内部通过 `new` 的方式创建依赖对象，导致类与类之间耦合度高、难以维护和测试。
- 依赖注入是控制反转的一种实现方式，核心思想是：**类不再自己创建依赖对象，而是通过外部容器在运行时将依赖注入进来**。
- Spring 通过 IoC 容器负责对象的创建与依赖管理，开发者只需声明依赖关系，由容器完成注入。

**依赖注入的三种方式**

- **构造器注入**

  - 通过构造方法传递依赖对象，确保依赖在对象初始化时就已经具备。

  - 推荐用于强依赖关系，且可实现不可变对象。

  - 示例：

    ```java
    @Service
    public class UserService {
        private final UserRepository userRepository;
    
        public UserService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }
    }
    ```

- **Setter 方法注入**

  - 通过 Setter 方法注入依赖，灵活性高，适用于可选依赖。

  - 缺点是对象可能被创建后未立即完成依赖注入。

  - 示例：

    ```java
    public class PaymentService {
        private PaymentGateway gateway;
    
        @Autowired
        public void setGateway(PaymentGateway gateway) {
            this.gateway = gateway;
        }
    }
    ```

- **字段注入**

  - 直接在成员变量上使用 `@Autowired` 注解注入依赖，语法最简洁。

  - 缺点是依赖关系不明确，隐藏性强，不利于单元测试与解耦，不推荐用于生产。

  - 示例：

    ```java
    @Service
    public class OrderService {
        @Autowired
        private OrderRepository orderRepository;
    }
    ```

**依赖注入的优势**

- 解耦依赖关系，提高可维护性和可测试性
- 实现代码复用，简化对象创建和管理
- 容器统一管理生命周期，提升系统一致性

**实际开发中的建议**

- 推荐使用构造器注入，便于单元测试与不可变对象设计
- Setter 注入适用于可选依赖或存在默认值
- 字段注入仅用于临时测试或特殊场景，正式开发中应尽量避免

### 如果让你设计一个SpringIoc，你觉得会从哪些方面考虑这个设计？

1. **Bean 的生命周期管理**
   - 需要设计 Bean 的创建、初始化、销毁等生命周期的管理机制。
   - 可考虑使用工厂模式和单例模式来创建 Bean 实例。
   - 生命周期的回调点应可扩展，例如支持初始化方法和销毁方法的回调。
2. **依赖注入机制**
   - 需要实现属性注入、构造器注入、Setter 方法注入等方式。
   - 可通过反射机制动态注入依赖对象。
   - 支持 XML 配置、注解驱动或 Java 配置类注入。
3. **Bean 的作用域支持**
   - 支持多种 Bean 的作用域，如：
     - 单例（singleton）
     - 原型（prototype）
     - 会话（session）
     - 请求（request）
   - 可使用 Map 存储不同作用域下的 Bean 实例。
4. **AOP 功能支持**
   - 需要支持 AOP 功能，用于实现横切逻辑的解耦。
   - 可基于动态代理机制实现 AOP，例如：
     - JDK 动态代理
     - CGLIB 字节码生成
   - 实现切面、通知、连接点、切入点等核心概念。
5. **异常处理机制**
   - 需要提供异常处理能力，增强容器健壮性。
   - 应考虑以下异常类型：
     - Bean 创建失败
     - 注入失败
     - 方法执行异常
   - 可使用 try-catch 捕获关键流程异常，输出清晰错误信息。
6. **配置文件加载机制**
   - 支持从不同的配置源加载 Bean 配置信息，如：
     - XML 配置
     - 注解配置（如 `@Component`, `@Autowired`）
     - Java 配置类（如 `@Configuration`, `@Bean`）
   - 提供灵活的配置解析模块，支持多种配置格式的扩展性。

### Spring AOP主要想解决什么问题？

1. **Spring AOP 的本质目的**
   - AOP（面向切面编程）不是替代面向对象的核心思想，而是一种**补充机制**。
   - 它主要针对“横切关注点”的处理，解决的是：
     - 与核心业务无关但又**重复出现在多个模块中的逻辑**
     - 如：日志记录、事务管理、安全校验、性能监控等
2. **为什么需要 AOP**
   - 在传统面向对象开发中，这些横切逻辑往往分散在多个类中，造成代码重复与耦合度上升。
   - AOP 提供了统一的方式，将这些“横切逻辑”从业务代码中分离出来，集中处理，做到**逻辑分离、增强解耦**。
3. **举例说明（日志场景）**
   - 假设系统中有多个类（如 ClassA、ClassB、ClassC），都需要在方法执行时记录日志。
   - 如果每个类都手动编写日志逻辑，会导致：
     - 重复代码
     - 耦合日志逻辑与核心业务逻辑
   - 使用 AOP 后，可以统一定义一个日志切面（Aspect）并织入方法执行的切点，实现统一的日志增强处理。
4. **AOP 的核心结构与工作方式**
   - **Aspect（切面）**：模块化横切逻辑的类
   - **Advice（增强）**：切面中实际增强执行的逻辑方法
   - **JoinPoint（连接点）**：程序运行中的可插入点（如方法执行）
   - **Pointcut（切入点）**：定义哪些连接点应被织入增强
   - **Weaving（织入）**：将 Advice 应用于目标对象的实际过程
5. **Spring AOP 的切点粒度**
   - Spring AOP 的切入点粒度通常是方法级别（Method Execution）
   - 将多个增强（Advice）按顺序织入目标方法，执行流程为：
     - 进入切面前执行 Advice（如日志记录）
     - 调用目标方法（核心代码）
     - 方法执行后再执行其他 Advice（如事务提交）
6. **Spring AOP 的意义**
   - 实现**横切逻辑与核心业务代码的彻底解耦**
   - 降低系统耦合度，提升可维护性与可拓展性
   - 提供更清晰的模块边界，便于统一控制与功能复用

### Spring AOP原理了解吗？

1. **Spring AOP 的实现原理**
   - Spring AOP 是基于 **动态代理技术** 实现的。
   - 所谓动态代理，是指在**运行时动态创建代理对象**，并将增强逻辑织入目标方法调用过程中，从而达到不修改源代码即可增强功能的目的。
   - AOP 并不依赖编译期，而是在运行期生成代理对象实现切面逻辑。
2. **动态代理的核心机制**
   - 在执行目标方法时，AOP 会创建代理对象并拦截方法调用，在方法执行前后织入切面逻辑（如日志、事务等）。
3. **Spring AOP 支持的两种代理方式**
   - **基于 JDK 的动态代理**
     - 使用 `java.lang.reflect.Proxy` 类和 `InvocationHandler` 接口实现。
     - 要求：被代理的类必须**实现一个或多个接口**。
     - 特点：基于接口实现代理，优先使用，执行效率高。
   - **基于 CGLIB 的动态代理**
     - 当目标类**没有实现任何接口**时，Spring 会使用 CGLIB 库生成该类的子类作为代理类。
     - CGLIB（Code Generation Library）是一个第三方字节码增强库，通过**继承方式**创建代理。
     - 特点：对类进行继承增强，但不能代理 final 类和 final 方法。
4. **Spring 如何选择代理方式**
   - 默认情况下，**如果目标类实现了接口，Spring 会优先选择 JDK 动态代理**。
   - 如果目标类没有实现任何接口，Spring 会自动切换使用 CGLIB 实现代理。
   - 可通过配置强制使用 CGLIB，例如在配置类上加上 `@EnableAspectJAutoProxy(proxyTargetClass = true)`
5. **总结对比**
   - JDK 动态代理：基于接口，轻量高效，推荐优先使用。
   - CGLIB 动态代理：基于类继承，更强大但限制较多（不能代理 final 类/方法）。
   - Spring AOP 通过封装这两种代理机制，实现了对方法的增强拦截。

### 动态代理是什么？

1. **动态代理的定义**
   - 动态代理是 Java 提供的一种机制，在**运行时动态创建代理对象**。
   - 核心用途是在**不修改原始类代码**的前提下，对目标对象的方法调用进行拦截和增强。
2. **动态代理的两种实现类型**
   - **基于接口的代理（JDK 动态代理）**
     - 要求：目标类**必须实现至少一个接口**
     - 实现方式：
       - 使用 `java.lang.reflect.Proxy` 类创建代理对象
       - 实现 `java.lang.reflect.InvocationHandler` 接口定义增强逻辑
     - 调用过程：
       - 每个代理类实例关联一个 handler 对象
       - 调用目标方法时会被转发到 `invoke()` 方法中，由开发者实现增强逻辑
   - **基于类的代理（CGLIB 动态代理）**
     - 工具：使用 CGLIB（Code Generation Library） 字节码生成库
     - 要求：目标类**不需要实现接口**
     - 实现方式：
       - 通过继承目标类，创建其子类作为代理类
       - 在子类中重写方法实现增强逻辑
     - 特点：
       - 不依赖接口，适用于没有实现接口的类
       - 不能代理 final 类和 final 方法
3. **总结对比**
   - JDK 动态代理：
     - 基于接口
     - 效率更高
     - Spring AOP 默认优先选择
   - CGLIB 动态代理：
     - 基于继承
     - 功能更强，适用于无接口类
     - 依赖第三方库，限制较多

### 动态代理和静态代理的区别

1. **代理模式的定义**

   - 代理是一种常用的设计模式，用于为目标对象提供一个替身对象，用来控制对目标对象的访问。
   - 通过代理对象可以在不修改目标类的前提下增强功能，实现职责分离、逻辑扩展。

2. **静态代理**

   - 代理类在代码编写阶段就已经存在，通常由程序员手动编写或使用工具生成。
   - 代理类和目标类都需要实现同一个接口，委托调用目标类的方法。
   - 特点：
     - 编译时确定代理关系
     - 结构清晰，维护成本高，扩展性差
     - 每新增一个目标类，就要写一个新的代理类，重复代码多

3. **动态代理**

   - 在程序运行期间，通过反射机制动态生成代理类。
   - 代理类在运行时由 JVM 创建，不需要预先定义。
   - 特点：
     - 运行时生成代理类，减少重复编码
     - 结构灵活，适合处理大量接口下的多个实现类的增强
     - 常用于 AOP、事务、日志等横切逻辑场景

4. **两者对比总结**

   | 项目     | 静态代理                 | 动态代理                      |
   | -------- | ------------------------ | ----------------------------- |
   | 创建时机 | 编译期                   | 运行期                        |
   | 实现方式 | 人工或工具手动创建代理类 | 使用反射或字节码增强技术      |
   | 可扩展性 | 差，代理类多，重复代码多 | 强，逻辑统一封装，代码复用高  |
   | 应用场景 | 简单、固定结构           | 框架开发、AOP、RPC 动态增强等 |

### AOP实现有哪些注解？

1. **@Aspect**
   - 用于定义一个切面类。
   - 注解作用在类上，表示该类中定义的是横切逻辑（切面）。
2. **@Pointcut**
   - 用于定义切入点，指定要拦截的方法或方法集合。
   - 通常搭配表达式使用，例如 execution、within、@annotation 等。
   - 可复用，提高切面代码的可读性与维护性。
3. **@Before**
   - 表示前置通知，在目标方法执行**之前**执行增强逻辑。
   - 常用于日志打印、参数校验、权限检查等。
4. **@After**
   - 表示后置通知，在目标方法执行**之后**（无论是否成功）执行增强逻辑。
   - 可用于释放资源、记录操作日志等。
5. **@Around**
   - 表示环绕通知，在目标方法执行**前后**都可以插入逻辑。
   - 功能最强，可完全控制目标方法是否执行及返回结果。
   - 通常用于事务控制、性能监控等场景。
6. **@AfterReturning**
   - 表示返回通知，在目标方法**正常返回结果后**执行增强逻辑。
   - 可获取返回值并进行处理或记录日志。
7. **@AfterThrowing**
   - 表示异常通知，在目标方法**抛出异常后**执行增强逻辑。
   - 用于统一异常处理或记录异常信息。
8. **@Advice**（了解即可）
   - 表示通用的通知类型，是更底层的统一定义方式。
   - 实际开发中已被 `@Before`、`@After` 等注解替代使用。

### 什么是反射？有哪些使用场景？

1. **什么是反射机制**
   - 反射是 Java 提供的一种运行时机制，允许程序在运行时动态获取类的信息（如属性、方法、构造器）并进行操作。
   - 即使在编译阶段并不知道某个类的具体类型，也能在运行时创建实例、调用方法、访问字段。
2. **反射的四大常用能力**
   - **运行时类信息访问**
     - 获取类名、包名、父类、实现的接口、构造器等信息。
     - 使用方式：通过 `Class` 对象获取结构信息。
   - **动态对象创建**
     - 在运行时创建类的实例，即使类名未知。
     - 实现方式：
       - `Class.forName("类名")`
       - `clazz.newInstance()` 或 `constructor.newInstance()`
   - **动态方法调用**
     - 在运行时调用对象的任意方法，包括私有方法。
     - 使用 `Method` 类的 `invoke()` 方法执行。
   - **访问和修改字段值**
     - 即使是私有字段，也可以通过反射读写。
     - 使用 `Field` 类的 `get()` 和 `set()` 方法完成。
3. **反射在 Spring 框架中的应用**
   - **依赖注入与 IoC**
     - Spring 利用反射来实现核心功能之一：依赖注入。
     - 在应用启动时，Spring 会根据注解或配置文件，使用反射创建 Bean 并注入其依赖。
     - 示例场景：
       - 类中加上 `@Autowired`
       - Spring 通过反射解析注解、定位类型、创建实例并注入
   - **动态代理实现**
     - AOP 中的切面方法、日志记录、事务控制等都是通过反射结合动态代理实现的。
     - Spring AOP 使用 JDK 动态代理或 CGLIB，在运行时动态生成代理对象，并通过反射调用增强后的业务方法。
4. **反射机制的优点**
   - 提供框架底层的核心能力
   - 实现类解耦、模块动态装配
   - 支持配置驱动开发、代码增强与控制反转
5. **反射机制在架构中的意义**
   - 支撑了 IoC、AOP、注解驱动开发等核心特性
   - 体现出框架设计中的**松耦合、高内聚、灵活性**和**可扩展性**
   - 是 Java 框架编程能力的基础能力之一

### **spring是如何解决循环依赖的？**

1. **什么是循环依赖**
   - 指两个或多个 Bean 之间通过属性相互依赖，形成“闭环”。
   - 例如：
     - A 类依赖 B，B 类依赖 A
     - A 中注入了 B，B 中又注入了 A，造成 Bean 实例化过程中相互等待，无法完成注入
2. **循环依赖在 Spring 中的三种情况**
   - **构造器注入循环依赖**
     - 通过构造方法注入依赖，若产生循环依赖将导致创建时无法进行，**Spring 无法解决**，会抛出异常。
   - **Setter 方法注入 + 多例模式**
     - 即使通过 setter 注入，也因为是原型（prototype）模式，Bean 不会被提前缓存，**Spring 也无法解决**。
   - **Setter 方法注入 + 单例模式**
     - 是 Spring 唯一可以解决的循环依赖场景。
     - 原因是单例 Bean 创建过程中会进行**三级缓存提前暴露**，允许引用未初始化完成的对象。
3. **Spring 解决循环依赖的机制：三级缓存**
   - Spring 为了解决**单例 + Setter 注入**下的循环依赖，引入了三级缓存机制：
     - **一级缓存（singletonObjects）**
       - 存放完全初始化好的单例 Bean
     - **二级缓存（earlySingletonObjects）**
       - 存放提前暴露但未初始化完成的 Bean，供依赖注入时引用
     - **三级缓存（singletonFactories）**
       - 存放用于创建早期对象引用的 ObjectFactory（主要用来解决 AOP 动态代理场景）
   - 核心流程：
     1. **实例化 Bean**
        - 通过构造器或反射创建空对象，并放入三级缓存
     2. **属性注入前暴露引用**
        - 将对象提前暴露到二级缓存，使其他 Bean 能访问到（即使尚未初始化完成）
     3. **属性填充（依赖注入）**
        - 进行依赖注入时，从缓存中获取另一个 Bean 的引用，打破循环
     4. **初始化完成**
        - Bean 初始化后，转入一级缓存，表示正式创建完成
4. **总结**
   - Spring 只解决 **“单例 + Setter 注入”** 的循环依赖问题。
   - 对于构造器注入或多例模式的循环依赖，**Spring 不支持解决，直接抛出异常**。
   - 利用三级缓存机制，可以在 Bean 尚未初始化完成时暴露引用，从而保证循环依赖场景下程序正常运行。

### spring三级缓存的数据结构是什么？

1. **什么是 Spring 的三级缓存**
   - 为了解决 **单例 Bean 的循环依赖问题**，Spring IoC 容器在 Bean 创建过程中引入了三级缓存机制。
   - 本质上，三级缓存都是 `Map<String, Object>` 类型的数据结构，分别存储不同阶段的 Bean 实例或引用。
2. **一级缓存（singletonObjects）**
   - 存储内容：已经完全初始化完成的 Bean 实例，表示 Bean 创建完成且准备就绪。
   - 使用场景：用于从容器中正常获取 Bean。
   - 数据结构：`Map<String, Object>`，键为 Bean 名称，值为 Bean 实例。
   - 所在位置：`DefaultSingletonBeanRegistry` 类的 `singletonObjects` 属性。
3. **二级缓存（earlySingletonObjects）**
   - 存储内容：已经实例化但尚未完成属性注入、初始化等过程的 Bean。
   - 使用场景：在依赖注入阶段用于提前引用，避免死循环。
   - 数据结构：`Map<String, Object>`，键为 Bean 名称，值为 Bean 实例。
   - 所在位置：`DefaultSingletonBeanRegistry` 类的 `earlySingletonObjects` 属性。
4. **三级缓存（singletonFactories）**
   - 存储内容：用于创建 Bean 的 `ObjectFactory` 对象，可以生成 Bean 的早期引用。
   - 使用场景：在 Bean 正在创建过程中，如果被其他 Bean 所依赖，可通过该工厂创建临时引用。
   - 数据结构：`Map<String, ObjectFactory<?>>`，键为 Bean 名称，值为 ObjectFactory 对象。
   - 所在位置：`DefaultSingletonBeanRegistry` 类的 `singletonFactories` 属性。
5. **三级缓存的协作机制**
   - Spring 在创建 Bean 时会先将其放入三级缓存。
   - 若发现被依赖，就通过三级缓存中的 `ObjectFactory` 生成早期引用放入二级缓存。
   - Bean 完成初始化后会移入一级缓存，彻底完成生命周期。

### spring框架中都用到了哪些设计模式？

1. **工厂设计模式（Factory Pattern）**
   - Spring 使用工厂模式创建 Bean 对象。
   - 核心类包括：`BeanFactory`、`ApplicationContext`
   - 通过配置或注解方式创建、管理对象，解耦了对象的创建过程。
2. **代理设计模式（Proxy Pattern）**
   - Spring AOP 的核心就是基于代理模式实现的。
   - 通过 JDK 动态代理或 CGLIB 实现对目标对象的增强，例如：日志、事务、权限校验等横切逻辑。
3. **单例设计模式（Singleton Pattern）**
   - Spring 默认将 Bean 作为单例管理。
   - 容器在启动时创建并缓存单例 Bean，保证全局唯一，提高性能，节省资源。
4. **模板方法模式（Template Method Pattern）**
   - Spring 提供如 `JdbcTemplate`、`HibernateTemplate` 等类，封装数据库操作的固定流程。
   - 开发者只需实现部分定制逻辑，重用模板定义的骨架流程，符合模板方法的结构。
5. **包装器设计模式（Decorator Pattern）**
   - 在多数据源或多租户场景中，Spring 支持使用包装器动态切换数据库连接。
   - 允许在不改变原有对象的基础上增强其功能，增加了灵活性与可扩展性。
6. **观察者设计模式（Observer Pattern）**
   - Spring 的事件发布机制就是观察者模式的典型应用。
   - 类如 `ApplicationEventPublisher` 发布事件，`ApplicationListener` 响应事件，实现对象间松耦合通知机制。
7. **适配器设计模式（Adapter Pattern）**
   - Spring AOP 中的增强类（Advice）和通知器使用了适配器模式，将不同类型通知适配到统一的 AOP 接口。
   - Spring MVC 中的 `HandlerAdapter` 也是经典适配器实现，用于兼容不同类型的 Controller。

### Spring常用注解？

1. **通用组件注解**

   - **@Component**
      标识一个普通组件类，由 Spring 容器进行管理。是所有组件注解的泛化父注解。
   - **@Controller**
      标识一个控制层组件，主要用于 Web 层，配合 Spring MVC 使用。
   - **@Service**
      标识一个服务层组件，通常用于封装业务逻辑。
   - **@Repository**
      标识一个持久层组件，用于 DAO 数据访问层，同时具有持久化异常转换功能。

2. **依赖注入相关注解**

   - **@Autowired**
      根据类型自动注入 Bean，可用于构造器、方法、字段。默认按类型装配。

   - **@Qualifier**
      配合 `@Autowired` 使用，根据名称进行注入，解决多个 Bean 类型相同的问题。

   - **@Resource**（JDK 提供）
      默认按名称注入，也可指定 name 属性。兼容 JSR-250 标准。

   - **@Value**
      用于注入配置文件中的值，例如：

     ```
     @Value("${server.port}")
     private int port;
     ```

3. **配置相关注解**

   - **@Configuration**
      声明该类是一个配置类，等同于 XML 配置文件。
   - **@Bean**
      用于手动注册一个 Bean，配合 `@Configuration` 使用，返回的对象会被注册到容器中。
   - **@PropertySource**
      加载指定的外部配置文件，例如 `application.properties`。

4. **生命周期相关注解**

   - **@PostConstruct**
      用于方法上，表示该方法在 Bean 初始化完成后执行。
   - **@PreDestroy**
      用于方法上，表示该方法在 Bean 销毁前执行。

5. **AOP 相关注解**

   - **@Aspect**
      声明当前类是一个切面类。
   - **@Before / @After / @Around / @AfterReturning / @AfterThrowing**
      声明不同类型的增强通知，用于织入切点方法的不同阶段。
   - **@Pointcut**
      定义切入点表达式，标识哪些方法会被切入。

6. **Spring Boot 常用注解（补充）**

   - **@SpringBootApplication**
      核心组合注解，等价于 `@Configuration + @EnableAutoConfiguration + @ComponentScan`
   - **@EnableAutoConfiguration**
      开启 Spring Boot 的自动配置功能。
   - **@ComponentScan**
      指定 Spring 扫描的基础包路径，默认扫描启动类所在包及其子包。

### Spring 的事务在什么情况下会失效？

1. **未捕获异常**
   - 如果事务方法中抛出异常，但异常没有被捕获，或者异常被捕获但未重新抛出，事务无法感知，导致**事务不会回滚**。
   - 通常是因为异常未传递到事务边界之外。
2. **非受检异常**
   - Spring 默认**只对运行时异常（RuntimeException 及其子类）**进行事务回滚。
   - 如果抛出的是检查异常（如 SQLException），事务默认**不会回滚**，除非手动指定 rollbackFor。
3. **事务传播属性设置不当**
   - 如果多个事务方法嵌套调用，且传播属性设置不当，可能导致事务失效或传播逻辑异常。
   - 尤其注意方法内部调用带有 `@Transactional` 注解的方法，默认不会开启新事务。
4. **多数据源事务未统一管理**
   - 当项目中存在多个数据源，若事务管理器没有正确配置或同时存在多个 `@Transactional` 管理不同数据源，可能导致事务失效。
   - 需配合使用分布式事务框架（如 Seata）或 JTA。
5. **跨方法调用导致事务不生效**
   - 当一个事务方法内部调用了另一个带有 `@Transactional` 注解的方法，调用方没有注解时事务不会生效。
   - 原因：Spring AOP 默认基于代理，**同一个类内部调用不会经过代理增强逻辑**。
6. **事务注解在非 public 方法上**
   - `@Transactional` 注解**只能标注在 public 方法**上才有效。
   - 如果标注在 private、protected 或 default（包访问）方法上，Spring 不会为其创建事务代理。

### Spring的事务，使用this调用是否生效？

1. **不会生效**

   - 使用 `this` 调用类中带有 `@Transactional` 的方法，事务增强逻辑不会生效。

2. **原因分析**

   - Spring 的事务是通过 **AOP 代理对象** 实现的，只有通过代理对象调用方法，事务才能被拦截并生效。
   - `this` 代表的是当前类本身，绕过了 Spring 的代理机制，**直接调用不会触发事务拦截器链**，所以事务不生效。

3. **示例说明**

   ```java
   @Service
   public class UserService {
   
       @Transactional
       public void methodA() {
           // 应该开启事务，但如果通过 this.methodA() 调用将无效
       }
   
       public void methodB() {
           this.methodA(); // 不会触发事务
       }
   }
   ```

4. **正确做法**

   - **方式一：通过代理对象调用自身方法**

     ```java
     ((UserService) AopContext.currentProxy()).methodA();
     ```

     需配置开启暴露代理对象：`@EnableAspectJAutoProxy(exposeProxy = true)`

   - **方式二：将事务方法拆到另一个类中**

     ```java
     @Service
     public class AService {
         @Autowired
         private BService bService;
     
         public void doWork() {
             bService.txMethod(); // 此处通过代理调用，事务生效
         }
     }
     
     @Service
     public class BService {
         @Transactional
         public void txMethod() {
             // 正常事务逻辑
         }
     }
     ```

### Bean的生命周期？

1. **实例化阶段**
   - Spring 通过构造器创建 Bean 实例。此时仅调用构造方法，不涉及属性赋值。
2. **属性赋值阶段**
   - Spring 通过反射给 Bean 字段赋值，包括：
	- `@Value` 注入配置值
	- `@Autowired` / `@Resource` 依赖注入
		- InstantiationAwareBeanPostProcessor#postProcessProperties 具体区分两者不同行为的
3. **Aware接口回调阶段**- 名称工厂上下文
   - Spring 检查是否实现以下接口，并回调注入容器相关资源：
     - BeanNameAware：注入当前 Bean 的名称（`setBeanName` 方法）
	 - BeanFactoryAware：注入所属 BeanFactory 容器（`setBeanFactory` 方法）
     - ApplicationContextAware：注入所属的 ApplicationContext（`setApplicationContext` 方法）
4. **初始化前处理阶段（BeanPostProcessor 前置处理）**
   - 调用所有注册的 `BeanPostProcessor#postProcessBeforeInitialization()` 方法
   - 允许开发者在初始化前对 Bean 做增强处理（如 AOP、代理等）
5. **初始化阶段**
	* Bean 执行初始化逻辑，有三种机制：
		* 实现 `InitializingBean` 接口 → 自动调用 `afterPropertiesSet()`
		- 配置 `<bean init-method="initXx"/>`
		- 使用 `@PostConstruct` 注解的方法（由 `CommonAnnotationBeanPostProcessor` 支持）
6. **初始化后处理阶段（BeanPostProcessor 后置处理）**
   - Spring 再次调用 `postProcessAfterInitialization(bean, beanName)`
   - 通常用于增强功能，如 AOP 动态代理包装
7. **Bean 准备就绪交由容器管理**
   - 此时已完成：构造 → 属性注入 → Aware → 初始化 → 后置处理
8. **销毁阶段**
   - 容器关闭时，Spring 回收 Bean，执行清理逻辑：
    1. 实现 `DisposableBean` → 调用 `destroy()`
    2. 配置 `<bean destroy-method="cleanup"/>`
    3. 使用 `@PreDestroy` 注解的方法（同样由 `CommonAnnotationBeanPostProcessor` 支持）

### Bean是否单例？

1. **默认是单例**
   - Spring 中的 Bean 默认是单例模式（Singleton），即每个 Bean 定义在容器中只会创建一个实例，并会被缓存复用。
2. **单例模式的优势**
   - **性能高**：Bean 在应用启动时创建一次，后续请求重复使用。
   - **节省资源**：避免频繁创建、销毁 Bean，提高内存利用效率。
3. **支持多例模式（Prototype）**
   - Spring 允许通过配置将 Bean 设置为原型模式（多例）。
   - 方法：
     - 使用注解方式：`@Scope("prototype")`
     - XML 配置方式：`<bean id="xxx" class="xxx" scope="prototype"/>`
   - 原型 Bean 每次获取都会创建一个新实例。
4. **使用多例模式的场景**
   - Bean 是有状态的（如有成员变量），避免并发访问时线程不安全。
   - Bean 创建过程中涉及不共享的资源。
   - Bean 构造过程比较轻量，不会带来性能问题。
5. **使用单例 Bean 的注意事项**
   - 若单例 Bean 中包含可变状态字段（如成员变量），需要注意线程安全问题。
   - 尽量将单例 Bean 设计为 **无状态（stateless）**，或借助 `ThreadLocal`、并发工具类等解决并发问题。

### Bean的单例和非单例，生命周期是否一样？

1. **生命周期是否相同**

   - 不相同，**Spring 只管理单例 Bean 的完整生命周期**。
   - 对于原型 Bean，Spring 在**创建完成后立即交由调用方使用**，后续生命周期（如销毁）不再管理。

2. **对比总结**

   | 生命周期阶段   | 单例（Singleton）                                         | 多例（Prototype）                          |
   | -------------- | --------------------------------------------------------- | ------------------------------------------ |
   | **创建时机**   | 容器启动时创建（或首次请求时，取决于懒加载配置）          | 每次请求时新建一个实例                     |
   | **初始化流程** | 执行完整生命周期：注入属性、调用 Aware 接口、初始化方法   | 每次创建新实例也会执行初始化流程           |
   | **销毁时机**   | 容器关闭时销毁，由容器调用 `destroy()` 方法或指定销毁方法 | 容器**不管理销毁**，需调用方手动清理       |
   | **内存占用**   | 实例常驻内存，占用小，但需注意线程安全                    | 每次调用生成新对象，占用大，需注意资源释放 |
   | **适用场景**   | 无状态服务（如 Service、DAO 等）                          | 有状态对象（如临时会话、计算组件等）       |

3. **注意事项**

   - 原型 Bean 不适合作为自动注入到单例 Bean 中的成员变量，**生命周期不一致可能导致资源泄漏**。
   - 若确需注入原型 Bean，应使用 `ObjectFactory` 或 `Provider` 延迟获取。

### Spring Bean 的作用域有哪些？

1. **什么是作用域（Scope）**

   - Bean 的作用域定义了 Bean 的**生命周期范围和可见性**。
   - 不同作用域影响容器如何创建、缓存和销毁 Bean 实例，以及 Bean 是否被多个对象共享。

2. **常见作用域类型**

   - **singleton（单例，默认作用域）**
     - 每个 Spring 容器中只有一个 Bean 实例。
     - 应用启动时创建，整个生命周期中复用。
     - 适用于大多数无状态组件（如 Service、DAO）。
   - **prototype（原型）**
     - 每次获取时都会创建一个新实例。
     - Spring 仅负责创建，不负责管理销毁。
     - 适用于有状态组件，如临时对象、算法类。
   - **request（请求作用域）**
     - 每次 HTTP 请求创建一个新的 Bean 实例，仅 Web 应用有效。
     - 适用于根据请求参数或上下文信息生成的 Bean。
   - **session（会话作用域）**
     - 每个 HTTP 会话中存在一个 Bean 实例，仅 Web 应用有效。
     - 适用于存储用户会话相关信息的 Bean。
   - **application（应用作用域）**
     - 每个 ServletContext 中存在一个 Bean 实例。
     - 在整个 Web 应用范围内共享，适合缓存数据 Bean。
   - **websocket（WebSocket 作用域）**
     - 每个 WebSocket 会话中一个 Bean 实例，仅支持 WebSocket 环境。
     - 通常配合消息服务类组件使用。
   - **自定义作用域（Custom Scope）**
     - 可实现 Spring 的 `Scope` 接口，定义自定义的作用域逻辑。
     - 适用于框架级扩展或特殊业务场景。

3. **作用域配置方式**

   - **XML 配置：**

     ```
     <bean id="myBean" class="com.example.MyBeanClass" scope="singleton"/>
     ```

   - **注解方式：**

     ```
     @Bean
     @Scope("prototype")
     public MyBeanClass myBean() {
         return new MyBeanClass();
     }
     ```

4. **使用注意事项**

   - prototype 作用域的 Bean 注入到 singleton Bean 时，生命周期不一致，需使用 `ObjectFactory` 或 `Provider` 解决。
   - Web 相关作用域（如 request/session）需在 Web 环境中使用，需开启 Spring Web 支持。

### Spring容器存的是什么？

1. **Spring 容器中存储的主要是 Bean 对象**
   - 所有被 Spring 管理的对象都称为 **Bean**，它是应用中的各种组件（如服务、控制器、仓储等）在容器中的表示。
2. **Bean 的定义与来源**
   - Bean 可以通过以下方式声明和注册到 Spring 容器中：
     - XML 配置（老方式）
     - 注解方式（如 `@Component`、`@Service`、`@Repository` 等）
     - Java 配置类中的 `@Bean` 方法
     - Spring Boot 自动装配机制
3. **Spring 容器的核心职责**
   - **创建**：根据配置或注解创建 Bean 实例（构造器、工厂方法）
   - **初始化**：注入依赖、执行初始化方法、回调接口等
   - **管理**：维护作用域、生命周期、依赖关系等
   - **销毁**：在容器关闭时调用销毁逻辑（如 `destroy()` 方法）
4. **总结**
   - Spring 容器本质上是一个 **高级 Bean 管理工厂**，它不仅负责存放 Bean，还负责整个生命周期的控制与扩展。

### 在Spring中，在bean加载/销毁前后，如果想实现某些逻辑，可以怎么做

------

1. **使用 `init-method` 和 `destroy-method`（XML 配置方式）**

   - 在 XML 中通过 `<bean>` 标签设置：

     ```
     <bean id="myBean" class="com.example.MyBeanClass"
           init-method="init" destroy-method="destroy"/>
     ```

   - 在类中实现对应方法：

     ```
     public class MyBeanClass {
         public void init() {
             // 初始化逻辑
         }
         public void destroy() {
             // 销毁逻辑
         }
     }
     ```

2. **实现 `InitializingBean` 和 `DisposableBean` 接口**

   - 实现 Spring 的回调接口：

     ```
     public class MyBeanClass implements InitializingBean, DisposableBean {
         @Override
         public void afterPropertiesSet() {
             // 初始化逻辑
         }
     
         @Override
         public void destroy() {
             // 销毁逻辑
         }
     }
     ```

3. **使用 `@PostConstruct` 和 `@PreDestroy` 注解（推荐）**

   - 基于注解方式，简洁直观，推荐用于现代项目中：

     ```
     public class MyBeanClass {
     
         @PostConstruct
         public void init() {
             // 初始化逻辑
         }
     
         @PreDestroy
         public void destroy() {
             // 销毁逻辑
         }
     }
     ```

4. **使用 `@Bean` 注解的 `initMethod` 和 `destroyMethod` 属性**

   - 在 Java Config 中使用：

     ```
     @Configuration
     public class AppConfig {
         @Bean(initMethod = "init", destroyMethod = "destroy")
         public MyBeanClass myBean() {
             return new MyBeanClass();
         }
     }
     ```

5. **使用建议与优先级说明**

   - 推荐使用 **注解方式（@PostConstruct / @PreDestroy）**，简单清晰，易于维护。
   - 多种方式可以共存，但同一生命周期事件**只执行一次**。
   - Spring Boot 默认会处理 `@PostConstruct`、`@PreDestroy` 和 `@Bean(initMethod=...)` 的生命周期钩子。

### Bean注入和xml注入最终得到了相同的效果，它们在底层是怎样做的？

1. **核心结论**
   - 无论是注解注入（如 `@Autowired`）还是 XML 注入（如 `<property>`），它们底层都会被解析为 `BeanDefinition` 并注册到 Spring 容器中，由统一的依赖注入机制处理。
2. **注解注入的底层流程**
   1. **类路径扫描**
      - 容器启动时，会扫描指定的包路径，识别 `@Component`、`@Service`、`@Controller`、`@Repository` 等注解。
   2. **注册 Bean 定义**
      - 扫描到的类信息会转换为 `AnnotatedBeanDefinition` 并注册到 `BeanDefinitionRegistry` 中。
   3. **依赖注入**
      - 容器在实例化 Bean 时会检测字段、构造器或方法上的注解（如 `@Autowired`、`@Resource`）并执行注入。
      - 注入逻辑由 `AutowiredAnnotationBeanPostProcessor` 等 Bean 后处理器完成。
   4. **完成初始化**
      - Spring 创建并完成依赖注入后，生成完整 Bean 实例。
3. **XML 注入的底层流程**
   1. **配置解析**
      - 启动时通过 `XmlBeanDefinitionReader` 解析 XML 配置文件中的 `<bean>`、`<property>` 标签。
   2. **注册 Bean 定义**
      - 标签信息转换为 `GenericBeanDefinition` 注册到 `BeanDefinitionRegistry` 中。
   3. **依赖注入**
      - 在 `BeanDefinition` 中记录构造参数和属性，容器在创建实例时直接根据配置完成依赖注入。
   4. **完成初始化**
      - Bean 初始化完成后可调用初始化方法（如 `init-method`）等。
4. **注解注入与 XML 注入的底层共性**
   - **统一注册模型**：最终都生成 `BeanDefinition` 对象注册到同一个容器中。
   - **依赖注入原理一致**：本质都是由容器读取依赖元数据，通过反射调用属性或方法完成注入。
   - **扩展机制兼容**：都支持 Bean 后处理器如 `BeanPostProcessor`、`ApplicationContextAware` 等。

### **Spring 的扩展机制**

1. **BeanFactoryPostProcessor**
   - 容器实例化 Bean 之前调用。
   - 可用于修改 Bean 的定义（如属性、作用域等）。
2. **BeanPostProcessor**
   - Bean 实例化后、初始化前后执行。
   - 可用于对 Bean 做增强（如代理、属性修改等）。
3. **PropertySource**
   - 用于加载外部配置属性源（如 yml 文件、数据库）。
   - 便于统一配置管理。
4. **ImportSelector 和 ImportBeanDefinitionRegistrar**
   - 可根据条件动态注册 Bean 定义。
   - 实现配置类的模块化扩展。
5. **HandlerInterceptor（Spring MVC）**
   - 拦截请求执行前、执行中、执行后逻辑。
   - 常用于权限控制、日志打印、接口限流等。
6. **ControllerAdvice（Spring MVC）**
   - 全局异常处理、参数校验处理等。
   - 统一处理控制器层面的异常逻辑。
7. **Spring Boot 的自动配置**
   - 通过创建 `@Configuration` 配置类 + `spring.factories` 机制自动注入。
   - 实现框架或第三方库的自动装配能力。
8. **自定义注解**
   - 可以结合 AOP 或元注解机制，实现权限控制、日志记录、接口限流等横切逻辑。

### **Autoweird和Resource有什么区别？**
### **Spring架构演进**

## Spring MVC

### **MVC 分层介绍**

1. **MVC 全称**
   - Model（模型）、View（视图）、Controller（控制器）。
   - 是一种典型的软件架构设计模式，实现职责分离，方便代码维护与扩展。
2. **各层职责**
   - **View（视图层）**
     - 为用户提供界面，负责与用户进行交互。
     - 如 HTML、CSS、JS、Bootstrap 等前端页面展示技术。
   - **Model（模型层）**
     - 代表一个存取数据的对象或模块，通常是 Java POJO。
     - 分为两类：
       - 数据承载类 Bean（如 User），用于传递数据。
       - 业务处理类 Bean（如 Service、DAO），用于处理业务逻辑或数据库操作。
   - **Controller（控制器层）**
     - 负责接收用户请求，并调用 Model 层进行处理。
     - 再根据 Model 返回的结果，决定渲染哪个视图 View。
     - 实现视图与模型的解耦。
3. **MVC 流程步骤**
   1. 用户通过 View 页面向服务端发出请求（表单、超链接、AJAX 等）。
   2. Controller 接收请求，调用 Model 层业务逻辑类进行处理。
   3. Model 层根据业务逻辑处理后返回数据给 Controller。
   4. Controller 将结果封装并传给 View，最终渲染页面响应用户。

### 了解SpringMVC的处理流程吗？

1. **用户发送请求到 DispatcherServlet** 用户在浏览器发出请求，首先被前端控制器 `DispatcherServlet` 接收。它是整个 Spring MVC 的核心控制器，负责请求的统一调度。
2. **DispatcherServlet 查找处理器映射器（HandlerMapping）** DispatcherServlet 根据请求 URL，调用 HandlerMapping 查找对应的处理器（Handler）及其拦截器（Interceptors），封装为一个 `HandlerExecutionChain`。
3. **返回 HandlerExecutionChain 给 DispatcherServlet** HandlerExecutionChain 包含：
   - 目标处理器（Controller）
   - 一个或多个拦截器（HandlerInterceptor）
4. **DispatcherServlet 调用 HandlerAdapter 执行处理器** DispatcherServlet 不直接执行处理器，而是通过 HandlerAdapter 来执行。它负责适配各种类型的处理器（如基于注解的、接口的等）。
5. **HandlerAdapter 调用 Handler 执行业务处理** Handler（即 Controller）执行业务逻辑，并将处理结果封装为 `ModelAndView` 返回。
   - `Model`：封装业务数据
   - `View`：逻辑视图名
6. **HandlerAdapter 返回 ModelAndView 给 DispatcherServlet** 此时，控制权交还 DispatcherServlet，携带处理结果。
7. **DispatcherServlet 调用 ViewResolver 解析视图** ViewResolver 根据逻辑视图名（如 "home"）解析成具体的物理视图路径（如 `/WEB-INF/views/home.jsp`）。
8. **返回具体 View 对象** 解析后的 View 对象（例如 JstlView）被返回，准备渲染。
9. **DispatcherServlet 渲染视图** DispatcherServlet 将 `Model` 中的数据填充进 View 对象（比如传入 JSP 页面），进行视图渲染。
10. **响应结果返回给客户端** 渲染完成后，生成 HTML 内容，返回给浏览器，用户看到最终结果页面。

### Spring MVC 中 HandlerMapping 和 HandlerAdapter 的作用与流程

1. **HandlerMapping 的作用与原理**
   - 作用：将请求映射到对应的处理器（即 Controller）。
   - 功能：根据请求的 URL、请求参数等信息，找到能够处理该请求的 Controller。
   - 类型：Spring 提供了多种 HandlerMapping 实现，如：
     - `BeanNameUrlHandlerMapping`
     - `RequestMappingHandlerMapping`（常用）
     - `SimpleUrlHandlerMapping` 等
   - 工作流程：HandlerMapping 会根据请求信息（主要是 URL）查找匹配的 Controller，并返回对应的处理器对象（或其执行链 `HandlerExecutionChain`）。
2. **HandlerAdapter 的作用与原理**
   - 作用：负责调用具体的处理器（Controller）执行处理逻辑。
   - 功能：处理器可能有不同的类型和调用方式，HandlerAdapter 会根据处理器的类型进行适配并调用相应的方法。
   - 类型：Spring 同样提供了多个 HandlerAdapter 实现，如：
     - `SimpleControllerHandlerAdapter`
     - `HttpRequestHandlerAdapter`
     - `RequestMappingHandlerAdapter`（常用）
   - 工作流程：根据 Handler 的类型，选择一个合适的 HandlerAdapter 来调用它的处理方法。
3. **HandlerMapping 与 HandlerAdapter 协同处理请求流程**
   1. 客户端发出请求，由 **DispatcherServlet** 接收。
   2. DispatcherServlet 调用 **HandlerMapping**，根据请求 URL 等信息查找匹配的处理器（Controller）。
   3. DispatcherServlet 根据返回的处理器类型，选择合适的 **HandlerAdapter** 来调用处理器方法。
   4. Controller 执行逻辑后，返回 **ModelAndView**（包含视图名和模型数据）。
   5. DispatcherServlet 调用 **ViewResolver** 解析逻辑视图名为实际的视图对象。
   6. DispatcherServlet 渲染视图，返回结果给客户端。

## SpringBoot

### 为什么使用 Spring Boot

1. **简化开发**

   Spring Boot 提供了一系列开箱即用的组件、默认配置和自动装配机制，极大简化了项目的配置和开发流程。开发者可以专注于业务逻辑的实现，而无需投入大量时间在 XML 或繁琐配置上。

2. **快速启动**

   Spring Boot 支持通过内嵌的 Web 容器（如 Tomcat、Jetty、Undertow）快速启动应用，无需单独部署 WAR 包或手动配置服务器，启动部署步骤简洁，适合快速开发与迭代。

3. **自动化配置**

   Spring Boot 通过自动配置（AutoConfiguration）功能，能够根据项目中引入的依赖和常规开发习惯，自动配置常用组件（如数据库、缓存、Web 框架等），降低配置复杂度，减少重复代码，实现最佳开发实践。

4. **独立运行**

   Spring Boot 应用默认打包为可执行 JAR 文件，内嵌服务器后可以通过 `java -jar` 命令直接运行，具备良好的可移植性和部署便利性，适合容器化部署。

5. **微服务友好**

   Spring Boot 是 Spring Cloud 的基础，天然适合构建微服务架构，支持服务注册、配置中心、断路器、网关等常用微服务组件的快速集成。

### Spring Boot 比传统 Spring 更优秀的原因

1. **自动化配置简化开发流程**

   Spring Boot 提供自动配置（AutoConfiguration）功能，大量采用“约定优于配置”的设计思想，使得开发过程中常用组件（如数据库连接池、缓存、Web 框架等）可以在无需显式配置的情况下自动完成初始化，显著提升开发效率，减少配置负担。

2. **支持快速启动与模块化集成**

   Spring Boot 提供丰富的 Starter 模块（如 `spring-boot-starter-web`、`spring-boot-starter-data-jpa` 等），通过引入对应的依赖即可快速集成常用的框架和中间件（如数据库、消息队列、缓存等），极大提高了项目搭建和开发的效率。

3. **内嵌服务器、部署简便**

   Spring Boot 默认集成多种内嵌服务器（如 Tomcat、Jetty、Undertow），应用程序可直接打包为可执行的 JAR 文件，无需部署到外部 Web 服务器，只需一行命令即可运行（如 `java -jar xxx.jar`），部署运维更加轻量和灵活。

4. **天然支持微服务架构**

   Spring Boot 与 Spring Cloud 紧密结合，是微服务架构的基础组件。其模块化、自动化、独立运行等特性非常契合微服务应用的开发与部署需求。

### Spring Boot 中常用的设计模式

1. **代理模式**

   Spring AOP 使用代理模式增强方法（如事务、日志、权限等）的执行逻辑。支持静态代理和动态代理，Spring 默认使用动态代理（基于 JDK 或 CGLIB 实现）在运行时为目标类生成代理对象。

2. **策略模式**

   Spring AOP 中的代理方式（JDK vs CGLIB）就是通过策略接口和不同策略实现类进行动态选择的体现。常见的动态 Bean 实例创建也会通过工厂方法结合策略来实现动态注入。

3. **装饰器模式**

   Spring 使用装饰器模式增强已有功能。例如 `TransactionAwareCacheDecorator` 用于在缓存访问时加入事务感知能力，对数据库事务操作提供支持。

4. **单例模式**

   Spring 中的 Bean 默认是单例模式，整个容器中仅存在一个实例。Spring 通过 BeanDefinition 注册表 + 单例缓存（如 `singletonObjects` 的 HashMap）来管理 Bean 的唯一性。

5. **简单工厂模式**

   Spring 中的 `BeanFactory` 是简单工厂模式的典型体现，它根据配置创建和管理 Bean 实例，对外提供统一获取接口。

6. **工厂方法模式**

   Spring 中的 `FactoryBean` 是工厂方法模式的代表，它允许开发者自定义 Bean 的创建逻辑，框架通过调用工厂方法返回 Bean 对象，适用于复杂 Bean 的定制。

7. **观察者模式**

   Spring 的事件发布机制使用了观察者模式，包括：

   - `Event`：事件对象
   - `Listener`：事件监听器
   - `Publisher`：事件发布者 事件注册在 `ApplicationContext` 中，通过 `ApplicationEventMulticaster` 完成广播与监听器调用。

8. **模板方法模式**

   Spring 中很多类的实现都采用模板方法模式，如 `JdbcTemplate`、`RestTemplate`、`TransactionTemplate` 等，定义了处理流程的骨架，具体步骤由子类或回调函数来实现扩展。

9. **适配器模式**

   Spring MVC 中，`HandlerAdapter` 用于适配不同类型的 Controller（如注解方式、HttpRequestHandler、SimpleController 接口等）。通过统一接口调用不同实现，达到调用逻辑与实现解耦的目的。

### 如何理解 Spring Boot 中的“约定大于配置”

1. **核心思想**

   “约定大于配置”（Convention over Configuration）是 Spring Boot 的核心设计理念之一。它通过**合理的默认行为和项目结构约定**，大幅减少手动配置工作，提升开发效率，降低新手上手难度，同时增强项目的可维护性和一致性。

2. **自动化配置**

   Spring Boot 提供大量自动化配置能力，通过分析项目中的依赖和环境，自动推断并配置常见组件，开发者无需手动配置每个细节。

   示例：

   - 引入 `spring-boot-starter-web` 后，自动配置内嵌的 Tomcat 服务器与 Spring MVC；
   - 不再需要显式配置 DispatcherServlet、视图解析器等基础设施；
   - 无需书写繁琐的 XML 配置。

3. **默认配置**

   Spring Boot 为常见开发场景提供合理的默认值，例如：

   - 默认使用内嵌 Tomcat 作为 Web 服务器；
   - 默认的日志级别为 INFO，日志格式和输出位置已配置好；
   - 默认配置数据库连接池为 HikariCP；
   - 默认启用 JSON 响应、错误页面处理等。

   这些默认配置极大减少了“开箱即用”的成本，开发者可专注于业务开发，真正做到“只配置你特别想配置的”。

4. **约定的项目结构**

   Spring Boot 对项目目录结构有推荐约定，以提升团队协作效率：

   - 应用主类（包含 `main` 方法）放置在根包；
   - 控制器类通常放置于 `com.example.demo.controller`；
   - 业务逻辑类放在 `com.example.demo.service`；
   - 数据访问类放在 `com.example.demo.repository`；

   遵循这些结构，有利于 Spring Boot 自动扫描组件，也方便开发团队快速理解项目结构和模块职责。

### Spring Boot 自动装配原理详解

1. **什么是自动装配**

   自动装配（Auto Configuration）是 Spring Boot 最核心的能力之一，它基于 Spring Framework 的条件化配置（Conditional）机制和 `@EnableAutoConfiguration` 注解实现。Spring Boot 会根据项目引入的依赖，自动配置相应的 Bean，实现“开箱即用”。

2. **核心注解及其作用**

   - `@EnableAutoConfiguration`：Spring Boot 自动装配的核心注解，开启自动配置机制。
   - `@SpringBootConfiguration`：用于标识当前类为 Spring Boot 配置类，本质上是 `@Configuration` 的语义增强。
   - `@ComponentScan`：自动扫描 `main` 方法所在包及其子包中的所有组件（带注解的类）。
   - `@AutoConfigurationPackage`：将指定包下所有组件自动注册到 Spring 容器中。
   - `@Import({AutoConfigurationImportSelector.class})`：引入自动装配的入口类 `AutoConfigurationImportSelector`，其职责是筛选并导入需要自动配置的类。

3. **自动装配的实现机制**

   - Spring Boot 在启动时会加载 `META-INF/spring.factories` 文件，里面定义了所有可用的自动配置类（实现了 `AutoConfiguration` 接口的类）。
   - 这些自动配置类在运行时会被 `AutoConfigurationImportSelector` 扫描和导入到容器中。
   - 每个自动配置类通常都会被 `@ConditionalOn...` 系列注解修饰，如：
     - `@ConditionalOnClass`：类路径存在某个类时才生效；
     - `@ConditionalOnMissingBean`：容器中不存在某个 Bean 时生效；
     - `@ConditionalOnProperty`：某个配置项开启时生效；
     - 等等。
   - 满足条件的配置类将被实例化并加入 Spring 上下文中。

4. **AutoConfigurationImportSelector 的核心职责**

   - **扫描类路径**：扫描 `spring.factories` 中的所有自动配置类；
   - **条件判断**：判断每个配置类是否满足导入条件（基于 `@Conditional` 注解）；
   - **条件导入**：将满足条件的配置类导入到上下文中，完成自动装配；
   - 方法入口：`getCandidateConfigurations()`，返回所有候选的自动配置类；
   - 自动配置类本质上也是普通的配置类，只是由系统根据规则自动加载。

5. **自动装配常见应用场景**

   - 自动配置内嵌 Web 容器（Tomcat、Jetty、Undertow）；
   - 自动配置数据源、JPA、Redis、RabbitMQ、Security 等；
   - 自动配置日志（默认使用 Logback）；
   - 自定义配置项绑定（结合 `@ConfigurationProperties`）。

### Spring Boot 中常用的重要注解汇总

1. **入口类相关注解**
   - **@SpringBootApplication**
      标注主应用类，是 Spring Boot 应用的入口，包含了：
     - `@Configuration`：声明该类是配置类；
     - `@EnableAutoConfiguration`：开启自动装配机制；
     - `@ComponentScan`：开启组件扫描。
2. **组件类注解**
   - **@Component**：通用组件注解，标识一个被 Spring 管理的 Bean。
   - **@Controller**：标注控制器类，处理 Web 层请求（返回视图）。
   - **@RestController**：组合注解，等价于 `@Controller + @ResponseBody`，用于返回 JSON 数据。
   - **@Service**：标注服务类，代表业务逻辑层。
   - **@Repository**：标注 DAO 层组件，用于数据库操作，并支持异常转换。
3. **依赖注入与配置属性相关注解**
   - **@Autowired**：自动装配 Bean，可作用于构造方法、字段或方法。
   - **@Value**：注入配置文件中的属性值，例如 `${server.port}`。
   - **@ConfigurationProperties**（补充）：可绑定整个配置类，适合批量注入。
4. **请求映射相关注解（Controller 方法中使用）**
   - **@RequestMapping**：通用的请求映射注解，支持所有请求方式（GET/POST/PUT/DELETE）。
   - **@GetMapping**：简化 `@RequestMapping(method = RequestMethod.GET)`。
   - **@PostMapping**、**@PutMapping**、**@DeleteMapping**：分别对应 POST、PUT、DELETE 请求。
5. **配置相关注解**
   - **@Configuration**：将类声明为配置类，等价于 XML 配置中的 `<beans>`。
   - **@Bean**：在配置类中声明一个 Bean，Spring 会将其注册到容器中。

### 常用的 Spring Boot Starter 启动器说明

1. **spring-boot-starter-web**

   包含：

   - Spring MVC；
   - 内嵌 Tomcat；
   - Web 开发常用组件。

   用于快速构建 Web 应用，提供请求映射、控制器、REST 接口支持，是最常用的启动器之一。

2. **spring-boot-starter-security**

   集成了 Spring Security 的核心配置，提供认证、授权、会话管理等功能，帮助开发者快速实现安全控制。

3. **mybatis-spring-boot-starter**

   由 MyBatis 官方提供的 Starter，简化了 MyBatis 的集成过程。自动配置如下组件：

   - `SqlSessionFactory`
   - `MapperScannerConfigurer`
   - Mapper 接口扫描注册等

   方便进行数据库访问操作。

4. **spring-boot-starter-data-jpa**

   集成了 Hibernate 等 JPA 实现，适用于基于 JPA 的数据持久化开发。支持实体类与数据库表的映射、自动建表、数据操作等。

   配置数据库连接信息后，即可进行 ORM 持久化开发。

5. **spring-boot-starter-jdbc**

   提供基础的 JDBC 支持，适合直接使用 SQL 语句操作数据库的项目，不通过 JPA。

   包含数据源管理、事务管理等基础设施。

6. **spring-boot-starter-data-redis**

   用于集成 Redis，支持缓存和数据存储功能。

   - 默认使用 Jedis 客户端；
   - 也支持 Lettuce；
   - 自动装配 `RedisTemplate`、连接工厂等组件。

   配合 `application.yml` 配置连接信息即可使用。

7. **spring-boot-starter-test**

   用于测试相关功能，包含：

   - JUnit
   - Spring Test
   - AssertJ
   - Mockito 等测试框架

   支持单元测试与集成测试，是 TDD（测试驱动开发）的重要工具。

### 开发一个 Spring Boot Starter 的步骤

1. **拆分模块**
   - **自动配置模块**：负责实现功能和配置逻辑；
   - **Starter 模块**：对外提供统一依赖入口，依赖自动配置模块和所需三方库。
2. **编写自动配置类**
   - 使用 `@Configuration` 声明配置类；
   - 使用条件注解（如 `@ConditionalOnClass`、`@ConditionalOnProperty`）控制配置是否生效；
   - 使用 `@Bean` 注册功能组件；
   - 使用 `@ConditionalOnMissingBean` 允许用户自定义覆盖。
3. **编写配置属性类**
   - 使用 `@ConfigurationProperties` 实现用户配置项绑定；
   - 使用 `@EnableConfigurationProperties` 启用属性绑定类。
4. **注册自动配置类**
   - 在自动配置模块的 `resources/META-INF/spring.factories` 文件中添加配置，将自动配置类声明给 Spring Boot 框架识别加载。
5. **整合依赖并封装 Starter 模块**
   - Starter 模块中引入自动配置模块与必要依赖；
   - 对外只需引入 Starter 即可使用功能，无需关心内部细节。
6. **用户引入方式**
   - 引入 Starter 后，通过配置文件进行简单参数配置，即可开箱即用，符合 Spring Boot 的“约定优于配置”原则。

### Spring Boot 中如何开启事务

1. **使用 @Transactional 注解**

   在 Spring Boot 中，开启事务非常简单，**只需在方法或类上添加 `@Transactional` 注解**，Spring 就会自动在调用时开启事务。

   - 一般加在**服务层（Service）的方法**上；
   - 注解可以加在**方法**上，也可以加在**类**上（表示该类中所有 public 方法都具有事务性）；
   - 方法执行成功时自动提交事务，抛出异常时自动回滚。

2. **常见场景**

   例如用户注册、订单下单、支付等场景中涉及多步数据库操作时，需要保证要么全部成功、要么全部失败，这时就应使用事务控制。

3. **依赖要求**

   虽然 Spring Boot 默认已包含事务支持，但实际数据库操作通常还需引入如 JPA、JDBC、MyBatis 等相关 Starter，比如：

   - `spring-boot-starter-data-jpa`
   - `spring-boot-starter-jdbc`
   - `mybatis-spring-boot-starter`

4. **常见注意事项**

   - 方法必须是 **public** 的，Spring AOP 的事务是基于代理实现的；

   - 方法**不能是同类内部自调用**（会失效）；

   - 默认情况下，**遇到运行时异常才回滚**，如需手动指定请使用：

     ```
     @Transactional(rollbackFor = Exception.class)
     ```

   - 推荐放在 **业务逻辑明确、需要一致性的地方**，如注册、修改数据等操作中。

补充总结一句话：

> Spring Boot 开启事务 = `@Transactional` + 合适的依赖 + 避免自调用 + 异常回滚控制

### Spring Boot 为什么能做到“导入依赖就能直接使用”？

1. **起步依赖机制**

   起步依赖是一种特殊的 Maven 或 Gradle 依赖，它将多个常用库打包在一起。例如 `spring-boot-starter-web` 含有 Spring MVC、Tomcat 等构建 Web 应用所需的核心依赖。开发者只需引入一个 Starter，构建工具会自动拉取所有关联依赖，避免繁琐配置。

2. **自动配置机制**

   自动配置是通过 `@EnableAutoConfiguration` 注解触发的，Spring Boot 启动时会读取 `META-INF/spring.factories` 文件中配置的自动配置类。每个 Starter 对应一个或多个自动配置类，Spring Boot 根据类路径下是否存在相关依赖，自动创建并注册对应的 Bean。

3. **条件注解控制**

   自动配置类通常结合条件注解使用，只有在满足特定条件时才生效，避免与用户配置冲突。常见的条件注解包括：

   - `@ConditionalOnClass`：类路径中存在指定类时生效；
   - `@ConditionalOnMissingBean`：容器中不存在某个 Bean 时自动注入；
   - `@ConditionalOnProperty`：配置文件中某个属性满足条件时生效。

4. **默认行为可被覆盖**

   如果用户在 `application.yml` 或代码中自定义了配置项或 Bean，Spring Boot 会优先使用用户的定义，自动配置会失效，从而实现灵活可扩展的“约定优于配置”机制。

5. **最终效果**

   只要引入对应的 Starter，就可以直接使用相关功能模块，无需手动创建配置类或注册 Bean，大大提升开发效率和上手体验。

### Spring Boot 中过滤器和拦截器的区别

1. **所属规范**

   过滤器属于 Java Servlet 规范的一部分，通过实现 `javax.servlet.Filter` 接口定义；
    拦截器属于 Spring 框架提供的机制，通过实现 `HandlerInterceptor` 接口定义。

2. **执行顺序**

   过滤器在请求进入 Servlet 容器前执行，属于最外层；
    拦截器在请求到达 Controller 方法前后执行，位于 DispatcherServlet 之后。

3. **使用范围**

   过滤器对所有请求都生效，包括静态资源请求；
    拦截器只能拦截 Spring MVC 控制器（Controller）层的请求。

4. **依赖注入支持**

   过滤器本身不支持注入 Spring 容器中的 Bean，如需注入需通过桥接方式；
    拦截器支持直接注入 Spring Bean，开发体验更优。

5. **触发时机**

   过滤器方法 `doFilter` 在请求进入或返回时触发；
    拦截器包括：

   - `preHandle`：在 Controller 方法执行前执行；
   - `postHandle`：在 Controller 方法执行后执行（未异常时）；
   - `afterCompletion`：在请求执行完毕（包括异常）后执行。

6. **适用场景**

   过滤器常用于编码处理、日志记录、安全验证、全局请求预处理等；
    拦截器多用于业务相关逻辑控制，如权限校验、参数验证、请求追踪等。


### springboot自动装配是通过哪个注解开启的
* @SpringBootApplication
	 * @SpringBootConfiguration
	 + @EnableAutoConfiguration
	 + @ComponentScan

## Mybatis

### MyBatis 做得比较好的方面

1. **SQL 与代码分离，灵活可控**

   MyBatis 允许开发者手写 SQL，避免 ORM 自动生成 SQL 的不透明问题。SQL 通常写在 XML 中，执行逻辑清晰明确，有利于性能调优。

2. **强大的动态 SQL 支持**

   支持 `<if>`、`<choose>`、`<foreach>` 等标签，可根据参数动态拼接 SQL，避免硬编码字符串拼接，提高灵活性与可读性。

3. **支持自动映射与自定义映射**

   支持字段与对象属性之间的映射关系绑定，通过 `<resultMap>` 实现复杂对象、嵌套集合的映射；自动支持驼峰命名转换。

4. **插件扩展机制完善**

   支持通过插件机制拦截 SQL 执行过程，常用于实现分页、SQL 性能分析、日志记录、安全过滤等场景。

5. **与 Spring 无缝集成**

   可通过 `@MapperScan` 快速注册 Mapper 接口，配合 Spring 的事务控制与 Bean 管理机制，使用体验简洁高效。

### 使用 JDBC 连接数据库的步骤

1. **加载数据库驱动程序**

   在使用 JDBC 连接数据库之前，需通过 `Class.forName()` 方法加载对应数据库的驱动类，例如加载 MySQL 驱动类为 `com.mysql.jdbc.Driver`。不同数据库使用的驱动类名称不同。

2. **建立数据库连接**

   使用 `DriverManager.getConnection(url, username, password)` 方法创建连接。参数说明如下：

   - url：数据库连接地址，包含数据库类型、主机、端口、数据库名；
   - username：数据库用户名；
   - password：数据库密码。

3. **创建 Statement 对象**

   通过 `Connection.createStatement()` 方法获取 Statement 对象，用于发送 SQL 查询或更新语句到数据库。

4. **执行 SQL 查询或更新操作**

   - 使用 `executeQuery(sql)` 执行 SELECT 查询；
   - 使用 `executeUpdate(sql)` 执行 INSERT、UPDATE、DELETE 等更新操作。

5. **处理查询结果**

   对于查询操作，会返回 `ResultSet` 结果集。可以通过：

   - `next()` 方法遍历行；
   - `getXXX()` 方法获取每列的值（如 `getString()`、`getInt()`）。

6. **关闭连接**

   数据库操作完成后，必须依次关闭资源，释放连接：

   - 先关闭 `ResultSet`；
   - 再关闭 `Statement`；
   - 最后关闭 `Connection`。

### MyBatis 中 # 和 $ 的区别

1. #（占位符方式）
   - `#{}` 会被 MyBatis 解析为 SQL 的预编译参数，占位符形式为 `?`；
   - 执行时通过 `PreparedStatement` 的 `set` 方法绑定参数；
   - 可以有效防止 SQL 注入，提高安全性；
   - 执行效率高，适合用于传入普通参数值（如字符串、数字等）。
2. **$（字符串拼接方式）**
   - `${}` 会被 MyBatis 直接进行字符串拼接，参数值会原样插入 SQL；
   - 不经过预编译，不能防止 SQL 注入；
   - 适合用于动态拼接表名、列名、排序字段等 SQL 结构；
   - 使用时要特别小心，确保参数来源安全，防止注入风险。



### MyBatis 中运用的常见设计模式

1. **建造者模式**

   用于构建复杂对象，如：

   - SqlSessionFactoryBuilder
   - XMLConfigBuilder
   - XMLMapperBuilder
   - XMLStatementBuilder
   - CacheBuilder 等

2. **工厂模式**

   封装对象创建逻辑，如：

   - SqlSessionFactory
   - ObjectFactory
   - MapperProxyFactory

3. **单例模式**

   控制全局对象唯一性，如：

   - ErrorContext
   - LogFactory

4. **代理模式**

   MyBatis 实现核心机制的基础，如：

   - MapperProxy、ConnectionLogger 使用 JDK 动态代理；
   - executor.loader 包中使用 CGLIB 或 Javassist 实现延迟加载。

5. **组合模式**

   用于构建树形结构的 SQL 节点，如：

   - SqlNode 及其子类（例如 ChooseSqlNode）

6. **模板方法模式**

   父类定义通用流程，子类实现差异化步骤，如：

   - BaseExecutor、SimpleExecutor
   - BaseTypeHandler 及其子类，如 IntegerTypeHandler

7. **适配器模式**

   封装不同日志框架接口，如：

   - Log 接口对接 JDBC、Log4j 等日志框架的实现

8. **装饰器模式**

   用于缓存功能的增强，如：

   - cache.decorators 包中各类装饰器（装饰 Cache）

9. **迭代器模式**

   封装配置解析过程中的属性遍历逻辑，如：

   - 属性解析类 PropertyTokenizer 实现自定义的键值对迭代方式

### MyBatis Plus 和 MyBatis 的主要区别

1. **CRUD 操作简化**

   MyBatis Plus 提供内置的 `BaseMapper` 接口，封装了常用的 CRUD 方法（如 `selectById`、`insert` 等），无需手动编写 SQL，提高开发效率。

2. **代码生成器支持**

   MyBatis Plus 提供代码生成器，可根据数据库表结构自动生成实体类、Mapper 接口及 XML 映射文件，减少手动编写工作量。

3. **通用方法封装**

   封装了条件构造器、排序、分页、更新等通用操作方法，无需重复造轮子，简化业务逻辑代码，提升开发效率。

4. **内置分页插件**

   提供分页插件，支持多种数据库分页查询，开发者无需手写分页 SQL，也无需手动封装分页逻辑。

5. **多租户支持**

   提供多租户插件，可支持多租户数据隔离场景，方便在 SaaS 系统中进行用户级数据隔离。

6. **注解支持增强**

   增强了注解能力，如 `@TableName`、`@TableField` 等，支持通过注解完成实体类与数据库字段的映射，减少对 XML 的依赖。

## SpringCloud

### Spring Cloud 和 Spring Boot 的区别

1. **定位不同**

   Spring Boot 是用于快速构建单体应用的开发框架，简化了 Spring 应用的开发和部署流程。
    Spring Cloud 是构建微服务系统的工具集，用于解决分布式系统中的服务治理问题。

2. **功能侧重点不同**

* Spring Boot 主要关注于应用的开发效率，如自动配置、起步依赖、嵌入式服务器等。
* Spring Cloud 侧重于微服务架构中的系统级功能，如服务注册与发现、负载均衡、断路器、网关、配置中心等。

3. **使用方式互补**

   两者可以组合使用：

   - 使用 Spring Boot 构建微服务中的每个服务；
   - 使用 Spring Cloud 提供服务间的通信和治理能力，实现完整的微服务架

### 微服务架构中的常用组件

1. **注册中心**

   注册中心用于管理微服务节点的注册与发现，解决了“如何发现新节点以及检查节点的运行状态”的问题。每个服务在启动时会将自身的地址、端口等信息注册到注册中心，注册中心也会定期检查节点是否存活，并将可用服务列表提供给消费者。

2. **负载均衡器**

   负载均衡器解决了“如何发现服务及负载均衡如何实现”的问题。它会从注册中心获取可用服务节点列表，并基于一定策略选择一个节点进行调用。常见策略包括轮询、权重、最小连接数等，保障调用效率与稳定性。

3. **服务通信**

   服务之间通信解决了“服务间如何进行消息通信”的问题。通常采用轻量级的 HTTP 协议或 RPC 协议。Spring Cloud 提供了 RestTemplate 和 Feign 两种通信方案，简化了服务调用的实现。

4. **配置中心**

   配置中心解决了“如何集中管理各节点的配置文件”的问题。在微服务架构中，每个服务可能部署多个实例，配置项容易出现不一致。通过配置中心统一管理配置，实现动态更新、版本控制等功能，避免频繁重启与重复维护。

5. **集中式日志系统**

   日志系统解决了“如何集中查看各节点的调用链路与数据分析”的问题。通常会将各服务日志集中发送到日志平台（如 ELK、EFK），便于异常分析、链路追踪、问题定位和性能调优。

6. **服务监控**

   服务监控用于解决“如何掌握微服务运行状态和调用异常”的问题。通过采集指标如响应时间、调用成功率等，实现可视化监控与预警。常用工具有 Spring Boot Actuator、Prometheus、Grafana 等。

7. **服务保护**

   服务保护解决了“如何避免服务雪崩和系统故障扩散”的问题。通过断路器、限流、隔离等机制保护服务的稳定性。例如 Hystrix、Sentinel 等组件用于自动熔断异常服务，防止故障级联。

### Spring Cloud Alibaba 微服务架构组件

1. **注册中心：Nacos**

   使用 Alibaba Nacos 实现注册中心，具备服务注册、服务发现、配置管理、动态更新等能力。Nacos 提供简单易用的界面和 API，支持快速接入微服务架构。

2. **负载均衡：Nacos 服务端负载均衡**

   Nacos 支持服务端负载均衡，不同于 Ribbon 的客户端负载均衡模式。服务消费者从 Nacos 获取服务列表后由 Nacos 控制返回节点，有助于实现更智能的服务路由。

3. **服务通信：Dubbo 和 Feign**

   Spring Cloud Alibaba 同时支持 Feign 和 Dubbo 两种服务通信方式。Feign 简洁易用，Dubbo 提供更强大的 RPC 功能，支持高性能远程调用，适合对性能要求高的系统。

4. **API 网关：Spring Cloud Gateway**

   网关用于统一接入、权限校验、请求转发、限流、监控等，是系统的流量入口。Spring Cloud Alibaba 使用的是与 Spring Cloud 同一套 Gateway 组件。

5. **配置中心：Nacos 配置管理**

   Nacos 除了注册中心功能，还提供配置中心功能，支持配置热更新、分环境管理，并可将配置信息持久化存储至指定数据库中。

6. **日志系统：ELK 集成**

   可使用阿里云日志服务或自行部署的 ELK 日志系统，实现日志统一采集、分析和监控，帮助定位问题与系统调优。

7. **链路追踪：Sleuth + Zipkin**

   在分布式系统中进行请求链路追踪，定位慢请求、异常请求，Spring Cloud Alibaba 采用与 Spring Cloud 相同的 Sleuth 与 Zipkin 实现分布式链路监控。

8. **服务熔断与限流：Sentinel**

   Sentinel 是阿里自研的高性能服务保护组件，支持熔断、限流、系统负载保护等功能。相比 Hystrix，Sentinel 功能更强大，控制粒度更细，并提供可视化管理界面。

### 常见的负载均衡算法

1. **简单轮询**

   按照顺序将请求依次分发给后端服务器，不考虑服务器当前的负载与性能，适用于服务器性能差异不大的场景。

2. **加权轮询**

   根据后端服务器性能设置权重，权重越高的服务器会被分配更多的请求。适用于不同服务器性能差异较大的情况。

3. **简单随机**

   将请求随机分配给后端服务器，每台服务器接收到的请求趋于均衡，适用于无状态服务。

4. **加权随机**

   为每台服务器设置权重，按权重概率随机选择服务器处理请求，使性能更强的服务器获得更多请求。

5. **一致性哈希**

   根据客户端 IP 或请求参数计算哈希值，映射到特定服务器。可保证同一客户端的请求总是落在同一台服务器上，适用于有状态服务或缓存系统。

6. **最小活跃数**

   实时统计每台服务器正在处理的请求数，将请求分配给当前活跃连接数最少的服务器，提高资源利用率，减少响应延迟。

### 如何实现请求始终路由到同一用户

1. **使用一致性哈希算法**

   根据请求的客户端 IP、用户 ID、请求参数等，通过哈希算法生成一个哈希值。

2. **哈希值取模映射服务器**

   使用生成的哈希值对服务器节点数取模，将结果映射到某一台后端服务器，实现用户请求的固定落点。

3. **实现效果**

   相同客户端或相同参数的请求，每次都被路由到同一台服务器，从而实现“一个用户一直被均衡到同一台机器”的效果。

### 服务熔断（Circuit Breaker）

1. **定义**

   服务熔断是一种用于防止服务雪崩效应的链路保护机制。其原理类似于保险丝，当某个服务节点响应异常或调用失败时，会自动中断对该服务的调用，防止问题进一步扩大。

2. **作用**

   当下游服务响应超时、失败次数过多或出现系统级问题时，熔断器自动触发熔断操作，阻止继续发送请求，以避免资源浪费、系统崩溃。

3. **使用场景**

   适用于高并发环境中，某个服务接口波动剧烈时，用于防止大面积调用失败造成级联故障。

4. **工作机制**

   熔断器会监控调用失败情况，如在单位时间内失败次数达到阈值（如 5 秒内失败 20 次），就会开启熔断机制。
    熔断后进入“短路”状态，快速返回失败响应，等待一段时间后进入“半开”状态测试服务是否恢复，最后再进入“闭合”状态恢复调用。

5. **Spring Cloud 实现**

   在 Spring Cloud 中可通过 Hystrix 实现熔断功能。Hystrix 监控请求状态，失败达到阈值后自动熔断该服务的调用链路。

### 服务降级（Service Degradation）

1. **定义**

   服务降级是指在系统负载过高或某些非核心服务不可用的情况下，主动限制资源使用，保证核心业务正常运行的一种策略。

2. **目的**

   在资源有限的情况下，通过对部分功能进行降级处理（如关闭推荐、延迟统计等），保护主业务流程稳定，提高系统整体可用性。

3. **使用场景**

   - 依赖的非关键服务异常或接口失败；
   - 某接口数据量大、响应慢，影响用户体验；
   - 处理高并发、突发流量压力。

4. **常见做法**

   - 返回默认数据或缓存数据；
   - 跳转到备用接口；
   - 提前准备 fallback 逻辑处理异常；
   - 配置限流、熔断联动触发降级处理。

5. **Spring Cloud 实现**

   常结合 Hystrix 使用，在服务调用失败时通过 fallback 方法返回降级处理结果，保障整体系统的稳定运行。