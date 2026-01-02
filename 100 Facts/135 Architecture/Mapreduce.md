---
tags:
- distributed-system
---
## 流程
![image.png](https://img.eryi.me/astro-blog/2025/11/392d7d0b9f9536bc8402802cd7a19ca9.png)

1. 输入文件分片 M片，每个块叫做input split；在机器上创建用户编写的程序副本
2. 一个 master，其余的都是 worker，master 来分配任务；M 个 Map 任务和 R 个 Reduce 任务将被分配
3. map worker 读取输入，解析 kv 对，传递给用户自定义的 Map()函数，Map()函数生成中间 kv 对，缓存在内存中
4. 分区函数将缓存的 kv 对分成 R 个 regins，周期性写到 local disk 上，在 disk 上的位置将会被回传给 master，master 将会把这些位置传送给 Reduce Worker；
5. Reduce Worker 接收到位置信息，使用 RPC 从 Map worker 读取到 kv 对。之后 Reduce Worker 将 key 进行排序后使用相同的 key 值的数据聚合在一起。
6. Reduce worker 遍历排序好的 kv 对，将这个 key 和相关的中间 value 值的集合传递给用户自定义的 Reduce()函数，输出被追加到所属分区的输出文件。
7. Map 和 Reduce 任务都完成后， master 唤醒用户程序，用户程序看到 mr 返回
用户得到的返回一般是 R 个输出文件，一般不需要合并成一个，可以作为另一个 MapReduce 的输入或其他应用中使用。

## Master 数据结构
* 每个 Map 和 Reduce 任务的状态
	* 空闲
	* 工作
	* 完成
* worker 机器标识
* 类似数据管道，当 Map 任务完成后，存到 disk 的位置和大小信息传递到 master，master 再增量传到 reduce
## 容错
### Workear故障
检测方式：master 周期性ping 每个 worker，约定时间范围内没有收到 worker 返回，就标记为失败
* 所有由这个失效的 worker 完成的 Map 任务被重设为初始的空闲状态，之后这些人物就可以被安排给其他的 worker
* 同理，正在运行 Map 或 Reduce 也会置为空闲状态
### Matser故障
* easy解决
	* checkpoint 方法：周期性将 master 内容写入 disk，故障就从上个检查点重新运行
	* 问题仅有一个 master 进程，失效恢复比较麻烦
* 进一步
	* 若 master 失效，就终止 MapReduce 运算，客户可以检查到这个状态，并且根据需要重新执行 MapRedece
* 用户的 map 或 reduce 函数是非确定性，mapreduce 也可以容忍。

## GFS
* GFS Google File System 分布式文件系统
* 输入数据放在集群中机器的 disk 钟，由 GFS 管理
* 调度时会尽量将一个 Map 任务调度在包含相关输入数据拷贝的机器上执行，因此网络带宽消耗较小

## 备用任务
* 当一个MapReduce操作接近完成的时候，master调 度备用（backup）任务进程来执行剩下的、处于处理中状态（in-progress）的任务
* 当一下两种情况发生后，运行该任务的进程终止
	* 原始进程自己完成并报告给 Master
	- 备用进程完成并报告给 Master
## 技巧
### Combinera函数
* 就是在 map worker中先合并一次
* 一般情况下，Combiner和Reduce函数是 一样的。Combiner函数和Reduce函数之间唯一的区别是MapReduce库怎样控制函数的输出。Reduce 函数的输出被保存在最终的输出文件里，而Combiner函数的输出被写到中间文件里，然后被发送给 Reduce任务

### Side-effects
* 开发者可能需要中间文件来进行旁路输出，使用`writer`将该操作变为原子和幂等的，如首先把结果写到一个临时 file 里，输出全部数据后，使用系统级的原子操作`rename`重新命名该文件
* 若产生多个输出文件，mr 没有提供 [2PC](分布式事务.md) 之类的原子操作，因此对于 会产生多个输出文件、并且对于跨文件有一致性要求的任务，都必须是确定性的任务