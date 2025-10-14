
| domain | layer   | type  |
| ------ | ------- | ----- |
| #java  | #spring | #fact |

##  [[事务]]管理器（Transaction Manager）
* Spring通过事务管理器来管理事务的生命周期。
* 事务管理器负责开启事务、提交事务、回滚事务等操作，Spring提供了多种事务管理器，适应不同类型的数据库和应用场景。
## Spring事务管理机制
### 声明式事务管理
* 即@Transactional 注解，声明式事务管理建立在 AOP 之上，本质通过 AOP 对加了直接的方法前后进行拦截
- 常见的`@Transactional`属性：
    - `propagation`：指定事务传播行为。默认为`REQUIRED`。
    - `isolation`：指定[事务的隔离级别](事务)。默认`DEFAULT`。
    - `timeout`：指定事务的超时时间。
    - `readOnly`：指定事务是否只读。若是只读事务，可以优化数据库操作性能。
    - `rollbackFor`：指定哪些异常会触发事务回滚，默认回滚`RuntimeException`和`Error`。
    - `noRollbackFor`：指定哪些异常不会触发回滚。
#### 优点
* 精简代码，自动对事物进行开启提交以及回滚，对代码没有侵入性
#### 缺点
* 最小粒度为方法，要给一部分代码块加事务则需要提取为方法
* 开发者容易忽略该注解，导致若在实务方法里面假如 RPC 消息发送，缓存更新等操作时
	* 1. 这些操作无法回滚
	* 2. 远程调用拉长失误持续时间，db 连接一直被占用，类似操作过多的话就会导致数据库连接池耗尽
### 编程式事务管理
* 编程式事务管理通过直接在代码中显式地管理事务的生命周期，通常在事务的开启、提交和回滚等操作时由开发人员进行手动控制。编程式事务管理通常是通过`PlatformTransactionManager`来实现的。
* 使用方式
	* a. 使用`TransactionTemplate`：  
	* b. 使用`PlatformTransactionManager`：  

## Spring事务关键特性
### Spring事务传播机制
* Spring通过事务管理器和AOP（面向切面编程）引入了事务传播机制，让开发者可以灵活控制事务如何在方法之间传播
* 事务传播行为决定了一个事务是否应该与另一个事务共享，或者是否应该创建一个新的事务。
	- **`REQUIRED`**：默认行为，若当前存在事务，则加入当前事务；若不存在事务，则新建事务。
	- **`REQUIRES_NEW`**：总是创建一个新的事务，若当前存在事务，则挂起当前事务。
	- **`NESTED`**：支持嵌套事务，当前事务会创建一个嵌套的事务，提交或回滚时嵌套事务的操作会被包括在外部事务中。

## @Transactional事务失效原因？

 1. `@Transactional`应用在非`public`修饰的方法上
	 * `@Transactional`基于spring aop实现，aop 又是基于 Java [[动态代理]]实现的，动态代理有氛围两种，JDK 动态代理是基于反射实现的，因此无法获取非 public 的字段；CGLIB可以
 2. `final`或`static`方法
	* 同样，[final方法](Java的final关键字)不能被子类重写，因此不能使用 CGLIB 动态代理
	* `static`方法是属于类的。代理机制依赖于实例化的代理对象，static 方法属于类不需要通过实例对象来调用。 
 3. 异常被`catch`捕获导致`@Transactional`失效
	- `@Transactional`默认会对运行时异常（`RuntimeException`）和错误（`Error`）进行回滚。如果异常被捕获（即使是`RuntimeException`），Spring事务管理也不会知道该异常发生，因此事务无法回滚，事务的失败无法被捕捉。
	- 解决：如果在方法内部捕获了异常并且不想让它影响事务的回滚，可以显式在`@Transactional`注解中设置`rollbackFor`属性来指定需要回滚的异常类型。

 4. 事务中使用了多线程
	 * `@Transactional`事务管理依赖于`ThreadLocal`来存储事务上下文。`ThreadLocal`变量是线程隔离的，每个线程有自己的事务上下文副本。如果在事务中使用了多线程操作，新线程中的操作不会共享原有事务，导致事务管理失效。
	* 解决：避免使用多线程或者将线程上下文传递

