---
tags:
- java
- juc
---
 在JLS规范中， 每个对象都天然带有一把 **Monitor 锁**；
 - `synchronized(obj)` 语义是：
    - **进入**：执行 `monitorenter` 字节码 → 拿到 obj 的 Monitor → 执行临界区。
    - **退出**：执行 `monitorexit` 字节码 → 释放 obj 的 Monitor → 唤醒等待线程。
- JLS 只规定了语义，不规定实现方式。
- 我们一般就是使用 hostspot 的虚拟机实现，而 Hostspot 采用了分层优化（即锁升级机制）
## 锁升级机制
### 无锁
* 默认状态；
* **mark word信息**
	* 放hashcode 和 GC 年龄等信息
	* lock bits=01 表示无锁
	- bias=0 表示不可偏向
	- PS：
		- JDK15 之前，是默认开启偏向锁的即 bias = 1；
		- JDK15 之后，默认不开启偏向锁即 bias = 0；即普通无锁对象
		- 
### 偏向锁
* **设计动机**：在大多数场景下， 锁只会被同一个线程多次获取
* **流程**
	* 第一次进入，将线程 ID 写入对象头
	* 之后进入，对比线程 ID 即可， 如果不一致
* **mark word信息**
	* hashCode 位被线程ID取代
	- lock bits=01 表示“偏向状态”
	- bias=1 表示可偏向
### 轻量级锁
* **设计动机**：让竞争的线程先尝试在用户态自旋，不立即挂起
* **作用流程**
	* 当线程进入synchronized(obj)中时
		* 在当前栈帧中创建Lock Record
		* CAS 尝试修改对象头（改 mark word 到 指向 Lock Record 的指针 + lock bits=00）
			* CAS成功 -> 获得锁
			* CAS 失败 -> 自旋或升级重量锁
				* 判断逻辑：“竞争强 + 自旋无望 / 需要阻塞等待或保存完整头信息” 就膨胀；否则先短自旋，能赢就保持轻量级，赢不了再膨胀。
* **mark word信息**
	* 保留指向新线程栈帧Lock Record（见][[JVM栈]]）的指针
		* 栈帧里的 Lock Record 保存之前的 *mark word* 和一个*指针*，指向正在被 synchronized 的对象。即这里有两个方向的指针
	* lock bits = 00，即轻量级锁状态

### 重量级锁
* **作用流程**
	* **monitorenter** 当线程进入synchronized(obj)中时
		* 发现对象头 Mark Word 指向 ObjectMonitor，进行判断
		* owner == null
			* CAS 把 `_owner` 设置为当前线程 → 加锁成功。
		* owner == 当前线程：
		    - 说明是 **可重入**，增加 `_recursions` 计数。
		- owner != 当前线程
		    - 说明锁被别的线程持有 → 当前线程进入 `_EntryList` 队列，并 **阻塞挂起**（挂起由操作系统互斥量/条件变量实现）。
	- **临界区**
		- 持有锁的线程继续执行同步代码块。
			- 期间如果调用 `wait()`，当前线程会释放 `_owner`，并进入 `_WaitSet` 队列，等待被 `notify/notifyAll` 唤醒。
	- **monitorexit**
		- 持有锁的线程执行 `monitorexit`，判断
			-  `_recursions > 0`
			    - 说明有重入 → 递减 `_recursions`，不释放锁
			-  `_recursions == 0`：
			    - 释放 `_owner`（置空）。
			    - 如果 `_EntryList` 非空：
			        - 选择一个等待线程唤醒（OS 层操作），让它来竞争 `_owner`。
			    - 如果 `_WaitSet` 有线程，必须等 `notify/notifyAll` 被调用才能移动到 `_EntryList` 参与竞争。
* **mark word信息**
	* 指针指向堆外的ObjectMonitor
	* 原来的 mark word 内容搬到了ObjectMonitor的 header 字段
	* lock bit = 10 即重量级锁
## ObjectMonitor
### 结构
```java
ObjectMonitor (native memory)
+--------------------------------------------+
| _owner     = 当前持有锁的线程 (T2)         |
| _EntryList = 等待锁的线程队列 (T1,T3,…)   |
| _WaitSet   = 调用 wait() 的线程队列       |
| _recursions= 重入次数                      |
| header     = displaced MarkWord (含hash)   |
+--------------------------------------------+
```
