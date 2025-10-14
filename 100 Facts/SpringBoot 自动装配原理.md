
| domain | layer   | type  |
| ------ | ------- | ----- |
| #java  | #spring | #fact |
* 自动装配核心思想：根据项目的实际情况自动注入适当的配置和Bean
#### 1. 引导类与启动
- **`@SpringBootApplication`**：
    - 包含 `@Configuration`、`@EnableAutoConfiguration` 和 `@ComponentScan`。
    - **`@EnableAutoConfiguration`** 启用自动装配机制。

#### 2. 自动装配的核心：`@EnableAutoConfiguration`
- 负责通过 `@Import(AutoConfigurationImportSelector.class)` 引入自动配置导入选择器。
	- **`AutoConfigurationImportSelector`** 类：
		- 负责读取 `META-INF/spring.factories` 文件，文件中记录了自动配置类的全类名。
		- 根据文件中的类名，加载对应的配置类。
#### 3. `spring.factories` 文件的作用
- `spring.factories` 文件位于项目的 `META-INF` 目录中，记录了自动配置类的全类名。
- 这些配置类中包含 Bean 定义，但是否将其 Bean 加载到 Spring 容器中，取决于是否满足条件。
#### 4. 条件注解：控制 Bean 加载
- 自动配置类中的 Bean 可以使用条件注解（如 `@ConditionalOnClass`）来判断是否加载。
    - **`@ConditionalOnClass`**：检查类路径中是否存在某个类（如 Kafka 的 `Producer` 类），只有类存在时，才会装配相应的 Bean。
    - 例如：
        ```java
        @Configuration
        public class FeatureConfig {
            @Bean
            @ConditionalOnClass(name = "org.apache.kafka.clients.producer.Producer")
            public KafkaProducer kafkaProducer() {
                // 仅在 Kafka Producer 类存在时才注册该 Bean
                return new KafkaProducer();
            }
        }
        ```
    - 这段代码中的 `KafkaProducer` 仅在 `org.apache.kafka.clients.producer.Producer` 类存在时才会被装配。
    
