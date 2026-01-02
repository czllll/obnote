---
tags:
- java
- juc
---
### Java线程状态种类

![image-20250212223314794](https://img.eryi.me/astro-blog/2025/09/e0a74c807f5a751bd45369935a56f412.png)

Thread.getState()获取当前线程状态
## NEW
  * **语义**：尚未启动的线程状态，即线程创建，尚未启动
  * `Thread t = new Thread();`
## RUNNABLE
* **语义**：线程正在执行，或等待 CPU 调度
* `t.start();`
## BLOCKED
* **语义**：线程尝试获取锁，但锁被其他线程占用
* `synchronized` 锁竞争
* EntryList：处于BLOCKED状态(锁阻塞)的线程，会被加入到entry set；
## WAITING
* **语义**：等待状态的线程正在等待另一线程执行特定的操作（如notify）, 线程主动等待，不会自动恢复
* `Object.wait()` / `LockSupport.park()`
* waiting的 线程
	* 显式调用 `wait()`，线程会进入 `WAITING`，等待 `notify()` 唤醒。
    * 显式调用 `LockSupport.park()`，线程会进入 `WAITING`，等待 `unpark()` 唤醒。
    * WaitSet ：处于WAITING状态(object.wait())的线程，会被加入到wait set；  
    * `ReentrantLock`（公平锁）内部使用 `LockSupport.park()`，线程进入 `WAITING`
    * `ReentrantLock`（非公平锁）内部使用 `LockSupport.park()`，线程进入进入队列会一直自旋，不会被挂
## TIME_WAITING超时等待
  * **语义**：等待一段时间后自动退出等待
  * `Thread.sleep()` / `Object.wait(time)`
## TERMINATED
  * **语义**：线程完成执行，中止状态
  * 线程执行完 `run()` 方法
