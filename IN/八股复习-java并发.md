#bagu 

## 多线程

### java里的线程和OS的线程一样吗？

* java底层会调用pthread_create 来创建线程，本质上java程序创建的线程就是和OS线程是一样的，是1对1的线程模型。

### Java线程安全哪三方面？

* 原子性

  * 定义：一个操作是不可分割的，要么全部执行成功，要么全部是被
  * 问题：
    * 多线程下可能导致结果错误
  * 解决
    * synchronized关键字
    * Lock锁
    * Atomic原子类
    * CAS操作

* 可见性

  * 定义：一个线程修改了共享变量，其他线程能立即看到修改后的值。
  * 问题
    * 一个线程修改了变量值
    * 其他线程可能看到的还是旧值

  * 解决
    * volatile关键字
    * synchronized关键字
    * Lock锁
    * final关键字（适用于不变量）

* 有序性

  * 程序执行的顺序按照代码的先后顺序执行
  * 问题
    * 代码重排序可能改变执行顺序户
    * 指令重排序可能影响多线程结果
  * 解决
    * volatile关键字
    * synchronized关键字
    * happen-before原则
    * Lock锁



### 保证数据一致性有哪些方案？

* 事务管理
  * 使用数据库事务来保证一组数据库操作要么全部提交成功，要么全部失败回滚
  * 通过ACID（原子性、一致性、隔离性、持久性）属性
* 锁机制
* 版本控制
  * 乐观锁方式，更新数据时记录数据的版本信息，避免同时对同一数据进行修改

### 线程创建方式有哪些？

* 继承Thread类
  * 用户自定义类继承java.lang.Thread类，重写run()方法，run()方法中定义了线程执行的具体任务。创建该类实例后，通过调用start()方法启动线程。
  * 使用this即可访问当前线程
* 实现Runnable接口
  * 若一个类已经继承了其他类，就不能再继承Thread类，此时可以实现java.lang.Runnable接口。
  * 实现Runnable接口要重写run()方法，然后将此Runnable对象作为参数传递给Thread类的构造器，创建Thread对象后调用其start()方法启动线程。
  * `Thread t = new Thread(new MyRunnable());`
  * 需要时使用Thread.currentThread()方法访问当前线程
* 实现Callable接口与FutureTask
  * Callable的call方法可以有返回值并且可以抛出异常。
  * 执行Callable任务需要将其包装进一个FutureTask，FutureTask实现了Runnable接口可以作为Thread的参数
  * 若想访问当前线程，必须调用Thread.currentThread()方法
* 使用线程池（Executor框架）
  * 通过Executors类的静态方法创建不同类型的线程池
  * 有点： 最大化CPU利用率和系统吞吐量

### 怎么启动线程？

* 通过Thread类的start()

### 如何停止一个线程的运行？

* 异常法停止
  * 线程调用interrupt()方法后，在线程的run方法中判断当前对象的interrupted()状态，若为中断状态则抛出异常，达到终端线程的效果
* 在沉睡中停止
  * 先将线程sleep，然后调用interrupt标记中断状态，interrupt会将阻塞状态的线程中断。
  * 会抛出中断异常，达到停止线程的效果
* stop()暴力停止
  * 线程调用stop()方法会被包里停止，方法已经弃用
* 使用return停止线程
  * 调用interrupt标记为中断状态后，在run方法中判断当前线程状态，若为中断状态则return



### 调用interrupt如何让线程抛出异常的？

* 每个线程都有一个boolean属性表示其中断状态，初始值为false
* 当一个线程被其他线程调用Thread.interrupt()方法中断时
  * 若该线程正在执行低级别的可中断方法（如Thread.sleep(),Thread.join()或Object.wait()）,则会解除阻塞并抛出InterruptedException异常。
  * 否则，Thread.interrupt()仅设置线程的中断状态，在该被中断的线程中稍后可通过轮询中断状态决定是否要停止当前正在执行的任务

### Java线程状态有哪些？

![image-20250212223314794](https://img.dirtsai.work/astro-blog/2025/02/e0a74c807f5a751bd45369935a56f412.png)

可以调用Thread.getState()方法来获取当前线程状态

* NEW
  * 尚未启动的线程状态，即线程创建，尚未启动
  * `Thread t = new Thread();`
* RUNNABLE
  * 线程正在执行，或等待 CPU 调度
  * `t.start();`
* BLOCKED
  * 线程尝试获取锁，但锁被其他线程占用
  * `synchronized` 锁竞争
* WAITING
  * 等待状态的线程正在等待另一线程执行特定的操作（如notify）, 线程主动等待，不会自动恢复
  * `Object.wait()` / `LockSupport.park()`
* TIME_WAITING超时等待
  * 等待一段时间后自动退出等待
  * `Thread.sleep()` / `Object.wait(time)`
* TERMINATED
  * 线程完成执行，中止状态
  * 线程执行完 `run()` 方法



### blocked和waiting的区别？

* 触发条件
  * BLOCKED
    * 当 `synchronized` 关键字锁被其他线程占用，线程**会进入 `BLOCKED`**，等待锁释放
  * WAITING
    * 显式调用 `wait()`，线程会进入 `WAITING`，等待 `notify()` 唤醒。
    * 显式调用 `LockSupport.park()`，线程会进入 `WAITING`，等待 `unpark()` 唤醒。
    * `ReentrantLock`（公平锁）内部使用 `LockSupport.park()`，可能让线程进入 `WAITING`，而不是 `BLOCKED`。
* 唤醒机制
  * 当一个线程被阻塞等待锁时，一旦锁被释放，线程将有机会重新尝试获取锁，若此时锁未被其他线程获取，那么线程可从BLOCKED变成RUNNABLE状态。
  * 线程在waiting状态中需要被显式唤醒



### waiting状态下线程如何恢复到runnable状态？

* `wait()` → `notify()` 或 `notifyAll()`
* `LockSupport.park()` → `LockSupport.unpark(thread)`
* `join()` → 目标线程执行完毕



### notify和notifyALL的区别？

* notify() 
  * 随机唤醒一个waiting线程，由JVM选择
* notifyALL()
  * 唤醒所有线程，开始竞争锁

### notify选择哪个线程

* 依赖JVM实现，有很多实现
  * 比较流行的是hotspot也是是FIFO



## 并发安全

### JUC包下你常用的类？

* 线程池
