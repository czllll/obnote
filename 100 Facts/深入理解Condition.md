
| domain | layer | type  |
| ------ | ----- | ----- |
| #java  | #juc  | #fact |

Condition实际上是JUC提供的一个接口，解决了 `Object.wait/notify/notifyAll` 的“协作”问题，并提供了远超后者的灵活性和精确性。
## wait/notify机制
是 Object 的方法，每个对象都有，是因为每个对象都有一个 monitor 监视器锁
调用这些方法的前提是线程必须已经持有这个对象的锁
### 方法机制hotspot语义下
#### wait()
- 线程调用 `obj.wait()`，必须持有锁。
- JVM 把它从 `_owner` 移除，并放入 `_WaitSet`。
- 完全释放锁。
- 挂起（`park`）。
#### notify()
- 当前线程持有锁。
- 从 `_WaitSet` 挑出一个线程，移到 `_EntryList`。
- 这个线程还不能马上执行，必须等待锁被释放。
#### notifyAll()
* 把 `_WaitSet` 里所有线程都移到 `_EntryList`，大家一起竞争锁。

### 局限性
相比于 Condition 而言， wait/notify是在对象级别的，一个对象只能有一个 waitList。 而 Condition 搭配 Lock 使用是锁级别的，一个锁可以有多个 Condition queue。同时 Thread（[[深入理解 Thread]]）也有 join()等操作线程的方法


## Condition
### 是什么？
- `Condition` 是 `java.util.concurrent.locks` 包里的接口。
- 和 `Lock` 配套使用，相当于 `Object.wait/notify` 的增强版。
- 一个 `Lock` 可以创建多个 `Condition`，每个 `Condition` 背后就是一个独立的 **条件队列**
### API
#### 等待方法
- `await()`：可中断等待。
- `awaitUninterruptibly()`：不可中断等待。
- `awaitNanos(nanosTimeout)`：返回剩余时间。
- `await(long, TimeUnit)`：返回是否在超时前被唤醒。
- `awaitUntil(Date)`：直到某个时刻。

#### 唤醒方法
- `signal()`：唤醒条件队列中的一个线程（通常够用）。
- `signalAll()`：唤醒该条件队列的所有线程（当不确定谁能继续时用）。

#### 实现原理
`Condition` 的方法（`await/signal` 等）语义上等价于 `wait/notify`，但 HotSpot 实现上不是直接调用 `Object.wait/notify`，而是基于 AQS+ LockSupport + 操作系统阻塞原语（futex/park/unpark） 自己实现的一套机制
* **调用链**
	*  `Condition.await()`
		1. 线程必须持有锁。
		2. 把当前线程包装成 `Node`，加入 `ConditionObject`（这是 AQS 的内部类） 的等待队列（单向链表）。
		3. 调用 `fullyRelease` 释放锁。
		4. 使用 **`LockSupport.park()`** 挂起线程。（LockSupport 实际调用 Unsafe类([[深入理解Unsafe]])实现）
		5. 被 `signal` 时，节点从 Condition 队列转移到 AQS（是 ReentrantLock的内部实现） 的同步队列，然后去竞争锁。
	* `Condition.signal()`
		1. 线程必须持有锁。
		2. 从 Condition 队列里移出一个等待节点。
		3. 调用 **`LockSupport.unpark(thread)`** 唤醒对应线程。
		4. 线程进入同步队列，等着重新获取锁。

