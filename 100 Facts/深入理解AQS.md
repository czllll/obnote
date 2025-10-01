
| domain | layer | type  |
| ------ | ----- | ----- |
| #java  | #juc  | #fact |

书：Concurrent Programming in Java: Design Principles and Patterns
设计模式：可复用面向对象软件的基础

全称AbstractQueuedSynchronizer，即抽象队列同步器。是一个构建锁和同步器的**抽象框架**。实现上来看是一个*抽象类*。
>*为什么使用抽象类而不是接口？*
>1. 接口在设计上不能拥有成员变量，所有的变量都是 public static final 的为类服务的。而我们需要 AQS 提供统一的状态管理，无需每一个实现的类去单独定义诸如`state`等的变量。
>2. 封装内部方法，Java9 以后接口可以有 private 的方法，但是 AQS 的年代还没有。
>3. AQS 是一个典型的[[模板方法模式]]的应用，将算法中某些可变的步骤延迟到子类中去实现。

## 核心思想
*资源状态*和*线程排队*的分离
### 资源状态State
AQS 内部只关心一个核心的 `int` 类型的状态变量 `state`。这个 `state` 具体代表什么由 AQS 的子类来定义：
	- `ReentrantLock` 中，`state` 表示锁的重入次数。0 表示未锁定，大于 0 表示已被某个线程锁定。
	- `Semaphore` 中，`state` 表示剩余的许可数量。
	- `CountDownLatch` 中，`state` 表示还需要倒数的计数值。
	-  AQS 使用 `volatile` 和 CAS 来保证对 `state` 的修改是线程安全的。

### 线程排队Queuing
当一个线程尝试获取资源（修改 `state`）失败时，AQS 会自动将该线程封装成一个节点（`Node`），并将其加入到一个先进先出 (FIFO) 的双向等待队列中。然后，该线程会被挂起（阻塞这里用的是 LockSupport.park() ），等待被唤醒。
## 内部结构
* `private volatile int state`
	* 同步状态
* `private transient Thread exclusiveOwnerThread`
	* 当前持有独占锁的线程
* 一个 CLH 队列
	* 一个虚拟的双向链表，用于存放等待获取资源的线程。
## 模式
* 独占模式
	* 资源在同一时间只能被一个线程持有。
	* `ReentrantLock`
	* 需要子类实现的方法：
		- `tryAcquire(int arg)`：尝试获取资源。
		- `tryRelease(int arg)`：尝试释放资源。
		- `isHeldExclusively()`：判断资源是否被当前线程独占。
* 共享模式
	* 资源在同一时间可以被多个线程持有
	* `Semaphore`, `CountDownLatch`
	* 需要子类实现的方法：
		- `tryAcquireShared(int arg)`：尝试获取共享资源。
			- 返回负数表示失败；
			- 0 表示成功但后续无法再获取；
			- 正数表示成功且后续其他线程也可能成功获取。
		- `tryReleaseShared(int arg)`：尝试释放共享资源。
## ConditionObject
ConditionObject 是AQS 提供的一个内部类，它是 [[深入理解Condition]] 接口的实现，用于支持 `await/signal` 机制
- **条件队列 (Condition Queue)**：每个 `ConditionObject` 内部都维护着一个独立的单向链表，我们称之为条件队列。
### 流程
- **`await()` 过程**：
    1. 当前线程必须先持有锁。
    2. 调用 `await()` 时，线程被封装成 `Node` 加入到**条件队列**的尾部。
    3. 线程**完全释放**它持有的锁（调用 AQS 的 `release` 方法，将 `state` 变为 0）。
    4. 释放锁之后，**唤醒** CLH 等待队列中的下一个线程，让它去竞争锁。
    5. 当前线程被挂起，在条件队列中等待。
- **`signal()` 过程**：
    1. 当前线程必须先持有锁。
    2. 调用 `signal()` 时，从**条件队列**的头部取出一个节点。
    3. 将这个节点从条件队列**移动**到 CLH 等待队列的尾部。
    4. 此时，被 `signal` 的线程并不会立即运行，它只是从“等待信号”的状态变成了“等待锁”的状态。它需要等待 `signal` 的线程释放锁后，按照 CLH 队列的规则被唤醒并重新竞争锁。



