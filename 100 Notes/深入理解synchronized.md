#java并发
 在JLS规范中， 每个对象都天然带有一把 **Monitor 锁**；
 - `synchronized(obj)` 语义是：
    - **进入**：执行 `monitorenter` 字节码 → 拿到 obj 的 Monitor → 执行临界区。
    - **退出**：执行 `monitorexit` 字节码 → 释放 obj 的 Monitor → 唤醒等待线程。
- JLS 只规定了语义，不规定实现方式。
- 我们一般就是使用 hostspot 的虚拟机实现，而 Hostspot 采用了分层优化（即锁升级机制）
## 锁升级机制
### 无锁
### 偏向锁
### 轻量级锁

## Monitor机制
* Monitor是JVM 内部的