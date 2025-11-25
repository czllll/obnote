---
tags:
- java
- spring
---
### 1. 入口启动、初始化配置
- `main` 方法：
    - Spring Boot 启动时，从 `main` 方法开始执行，调用 `SpringApplication.run(主类.class, args)`，该方法的主要作用是启动 Spring 应用并初始化 Spring 环境。
- 加载 `META-INF/spring.factories`：
    - Spring Boot 特有的机制，`spring.factories` 文件配置了自动化配置的初始化器（`ApplicationContextInitializer`）和监听器（`ApplicationListener`）。
    - 这些配置在应用程序启动时被加载，用于完成 Spring Boot 的一些扩展和初始化任务，如 `SpringApplicationRunListener` 和 `ApplicationContextInitializer`。

### 2. 创建应用程序上下文
- **`createApplicationContext()`**：
    - `SpringApplication` 在执行时，会创建一个 `ApplicationContext`（通常是 `AnnotationConfigApplicationContext` 或 `GenericWebApplicationContext`），该上下文是 Spring 容器的核心部分。
    - 在此过程中，会根据配置类（例如 `@Configuration` 注解的类）和扫描的包来创建和初始化 Bean 工厂。
### 3. 刷新应用程序上下文
- **`refresh()` 方法**：
    a. **配置工厂对象**：
    * Spring 会配置应用程序上下文，包括上下文类加载器、bean 工厂对象、`beanFactoryPostProcessor` 等。
    b. **注册并实例化 `BeanFactoryPostProcessor`**：
    - `BeanFactoryPostProcessor` 是在 Bean 的定义加载后，但实例化之前，执行的回调接口。Spring 会扫描配置文件或注解来初始化和执行这些处理器，修改应用上下文中 Bean 的定义。
    c. **注册并实例化 `BeanPostProcessor`**：
    - `BeanPostProcessor` 是在每个 Bean 的实例化和依赖注入完成后，执行的回调接口。它提供了一个拦截 Bean 初始化过程的机制，用于修改或增强 Bean 对象。
    d. **初始化一些与上下文有特别关系的 Bean 对象**：
    - 这一部分通常包括应用程序中与容器密切相关的组件，比如启动嵌入式服务器（Tomcat、Jetty 等），初始化 `WebApplicationContext`，准备环境变量和属性等。
    e. **实例化所有 Bean 工厂缓存的 Bean 对象**：
    - 在这一阶段，Spring 会根据上下文中配置的 Bean 定义，实例化所有的 Bean 对象，并进行依赖注入。
    f. **发布通知—上下文刷新完成**：
    - 一旦 Spring 容器完成初始化，`ApplicationContext` 会发布一个 `ContextRefreshedEvent`，通知所有的监听器（例如 `ApplicationListener`）启动过程已经完成。如果是 Web 应用，还会启动嵌入式服务器（如 Tomcat）。
### 4. 启动完成
- 发布 `ApplicationReadyEvent` 事件：
    - 当应用程序准备好并完全启动完成后，Spring Boot 会发布 `ApplicationReadyEvent` 事件，表示应用已经就绪，可以处理请求了。此时的状态标志着 Spring Boot 启动过程的完成。