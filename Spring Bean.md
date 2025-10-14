
| domain | layer   | type  |
| ------ | ------- | ----- |
| #java  | #spring | #fact |

## 是什么
是Spring容器管理的对象
## 作用域
Spring Bean 的作用域决定了 Bean 的生命周期和在容器中的实例化方式。
理论上来讲Bean 的作用域分为两种：
1. Singleton 单例作用域：
	- 每一次从IOC容器获取指定Bean对象时，都会返回同一个实例对象
	- 适用场景：无状态Bean，如工具类、配置类、服务类等
2. Prototype 原型作用域：
	- 每一次从IOC容器获取指定Bean对象时，都会返回一个新的实例对象
	- 适用场景：有状态Bean，需要隔离状态的场景
但在基于Spring框架下的Web应用中，是 Spring 在 Web 环境下的扩展
1. Request 请求作用域：
	- 针对每一次HTTP请求都会创建一个新的Bean，请求结束后销毁
	- 适用场景：HTTP请求级别的数据，如表单提交、请求参数处理
2. Session 会话作用域：
	- 针对Session会话，同一个Session共享同一个Bean实例，不同的Session则产生不同的Bean实例
	- 适用场景：用户会话数据，如登录信息、用户偏好设置
3. Application 应用作用域：
	- 整个Web应用中共享一个实例，类似于 ServletContext 级别
	- 适用场景：全局共享的配置或资源，如缓存，应用级配置
## 生命周期
- **实例化和属性赋值阶段**
    - Spring 通过构造器创建 Bean 实例，然后使用反射进行属性赋值。
    - 在这个阶段，如果定义了 `InstantiationAwareBeanPostProcessor`，它会处理 Bean 的属性注入。
- **`Aware` 接口回调阶段**
    - Spring 会检查是否实现了 `BeanNameAware`、`BeanFactoryAware`、`ApplicationContextAware` 等接口，注入容器信息。
- **初始化前处理（`BeanPostProcessor` 前置处理）**
    - 调用所有注册的 `BeanPostProcessor#postProcessBeforeInitialization()` 方法，允许对 Bean 进行预处理和增强。
- **初始化阶段**
    - 执行 Bean 的初始化逻辑，包括实现 `InitializingBean` 接口、配置的初始化方法、以及 `@PostConstruct` 注解的方法。
- **初始化后处理（`BeanPostProcessor` 后置处理）**
    - 调用 `postProcessAfterInitialization(bean, beanName)`，通常用于增强功能，如 AOP 动态代理包装。
- **销毁阶段**
    - 容器关闭时，执行清理逻辑，包括 `DisposableBean` 接口的 `destroy()` 方法、配置的销毁方法以及 `@PreDestroy` 注解的方法。