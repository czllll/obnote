
| domain | layer   | type  |
| ------ | ------- | ----- |
| #java  | #spring | #fact |

##  [[事务]]管理器（Transaction Manager）
* Spring通过事务管理器来管理事务的生命周期。
* 事务管理器负责开启事务、提交事务、回滚事务等操作，Spring提供了多种事务管理器，适应不同类型的数据库和应用场景。
## Spring事务管理机制
### 声明式事务管理
* 声明式事务管理是Spring最常用的事务管理方式，使用AOP来实现事务的管理。
* 通过配置注解或XML，开发人员可以非常简洁地在业务方法上指定事务的行为。

**a. 基于注解的事务管理**：  
通过`@Transactional`注解，Spring会自动为方法或类添加事务管理功能。

- **常见的`@Transactional`属性：**
    - `propagation`：指定事务传播行为。默认为`REQUIRED`。
    - `isolation`：指定事务的隔离级别。默认`DEFAULT`。
    - `timeout`：指定事务的超时时间。
    - `readOnly`：指定事务是否只读。若是只读事务，可以优化数据库操作性能。
    - `rollbackFor`：指定哪些异常会触发事务回滚，默认回滚`RuntimeException`和`Error`。
    - `noRollbackFor`：指定哪些异常不会触发回滚。

示例代码：

```java
@Service
public class UserService {

    @Transactional(propagation = Propagation.REQUIRED, isolation = Isolation.READ_COMMITTED, rollbackFor = Exception.class)
    public void createUser(User user) {
        // 执行数据库操作
    }
}
```

**b. 基于XML配置的事务管理**：  
除了注解方式，Spring还可以通过XML配置来声明事务管理。配置中定义`<tx:advice>`和`<tx:transaction-manager>`，使得方法调用时能够自动进行事务控制。

```xml
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="create*" propagation="REQUIRED" />
        <tx:method name="get*" read-only="true" />
    </tx:attributes>
</tx:advice>

<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource" />
</bean>
```

### 编程式事务管理（Programmatic Transaction Management）

编程式事务管理通过直接在代码中显式地管理事务的生命周期，通常在事务的开启、提交和回滚等操作时由开发人员进行手动控制。编程式事务管理通常是通过`PlatformTransactionManager`来实现的。

**a. 使用`TransactionTemplate`**：  
Spring提供了`TransactionTemplate`，它是一个高层API，简化了事务的控制。`TransactionTemplate`封装了事务管理器，提供了事务的执行和回滚功能。

```java
@Service
public class UserService {
    private final TransactionTemplate transactionTemplate;

    @Autowired
    public UserService(TransactionTemplate transactionTemplate) {
        this.transactionTemplate = transactionTemplate;
    }

    public void createUser(User user) {
        transactionTemplate.execute(status -> {
            // 执行数据库操作
            return null;
        });
    }
}
```

**b. 使用`PlatformTransactionManager`**：  
`PlatformTransactionManager`是Spring事务管理的底层接口，可以手动管理事务。开发者需要在代码中显式控制事务的开启、提交和回滚。

```java
@Service
public class UserService {
    @Autowired
    private PlatformTransactionManager transactionManager;

    public void createUser(User user) {
        DefaultTransactionDefinition def = new DefaultTransactionDefinition();
        def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);
        TransactionStatus status = transactionManager.getTransaction(def);
        try {
            // 执行数据库操作
            transactionManager.commit(status);
        } catch (Exception e) {
            transactionManager.rollback(status);
        }
    }
}
```

### Spring事务关键特性

## 事务传播行为
* 事务传播行为决定了一个事务是否应该与另一个事务共享，或者是否应该创建一个新的事务。
	- **`REQUIRED`**：默认行为，若当前存在事务，则加入当前事务；若不存在事务，则新建事务。
	- **`REQUIRES_NEW`**：总是创建一个新的事务，若当前存在事务，则挂起当前事务。
	- **`NESTED`**：支持嵌套事务，当前事务会创建一个嵌套的事务，提交或回滚时嵌套事务的操作会被包括在外部事务中。

#### 3. **事务回滚规则（Rollback Rules）**

默认情况下，Spring会对`RuntimeException`及其子类、`Error`进行回滚。开发人员可以通过`rollbackFor`和`noRollbackFor`属性指定自定义的回滚规则。

```java
@Transactional(rollbackFor = Exception.class)
public void createUser(User user) {
    // 如果抛出Exception或其子类的异常，将会回滚事务
}
```

#### 4. **事务的只读属性（Read-Only）**

Spring支持事务的只读属性，表示当前事务中的操作不会修改数据。对于只读事务，数据库可以进行优化，以提高性能。

```java
@Transactional(readOnly = true)
public List<User> getUsers() {
    // 执行查询操作
}
```

#### 5. **事务的超时（Timeout）**

Spring允许为事务设置超时时间，如果事务在规定时间内没有完成，Spring会自动回滚事务。

```java
@Transactional(timeout = 10)
public void createUser(User user) {
    // 执行操作，超时将会回滚
}
```

### **四、Spring事务的配置**

Spring事务管理器的配置依赖于具体的数据源。常见的事务管理器包括：

- **`DataSourceTransactionManager`**：用于基于JDBC的数据源事务管理。
    
- **`JpaTransactionManager`**：用于JPA（Java Persistence API）事务管理。
    
- **`HibernateTransactionManager`**：用于Hibernate事务管理。
    
- **`JtaTransactionManager`**：用于分布式事务管理。
    

#### 示例：

```xml
<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
    <property name="entityManagerFactory" ref="entityManagerFactory" />
</bean>
```

### **五、Spring事务的使用场景**

1. **多个数据库操作的组合**：确保一系列的数据库操作要么全部成功，要么全部失败。
    
2. **跨服务调用**：当多个服务涉及到数据操作时，使用Spring事务来保证数据一致性。
    
3. **分布式事务**：在微服务架构中，通过Spring的JTA事务管理器支持跨数据库或跨服务的事务管理。
    

### **总结**

Spring事务管理通过声明式和编程式两种方式提供了灵活且强大的事务控制。声明式事务管理简化了事务控制，开发人员只需要关注业务逻辑，而无需显式地管理事务的生命周期。编程式事务管理提供了更多控制和灵活性，适用于一些复杂的事务需求。Spring支持事务的传播行为、隔离级别、回滚规则等特性，可以帮助开发人员应对各种复杂的事务场景。