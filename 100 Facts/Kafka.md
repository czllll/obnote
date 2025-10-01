
| domain      | layer  | type  |
| ----------- | ------ | ----- |
| #middleware | #kafka | #fact |

* 基本概念
* 常见问题
	* 消息积压
	* 事务消息
	* kafka 分区 Leader 选举过程
	* kafka Controller 选举过程


## 基本概念
### Topic
Topic是一个逻辑概念
* 生产者发布的消息是需要指定一个Topic的
* 消费者是从它所指定订阅的 “主题”Topic 中去拉取的
* 一个主题Topic被分成多个Partition分区
### Patition
![image.png](https://img.dirtsai.work/astro-blog/2025/09/b5de0e64374d6466b98377475941898f.png)
- Partition分区 是最小的存储单元，掌握着一个Topic的部分数据。
- 每个 Partition 分区都是一个单独的 log 文件，每条记录都以追加的形式写入。
- 一个 Topic 的所有 Partition是分布在多个不同的Broker中的，目的是提高容错率、提高消息的消费能力，所有 Partition 的数据的并集就是所有数据
- 一个 Partition 会生成多个副本Replica（replication.factor = 3），并且把它们分散存储在不同的 Broker 中
##### 选举过程

### OffSet
在 kafka 中一共有两类 offset，一类是消息 offset，一类是消费 offset
#### 消息offset
**定义**：Kafka 在Partition 内给每条消息分配的递增编号，从 0 开始，追加一条消息就 +1
**特点**：
	- 不可变，即使消息过期或被清理也不会重用。
	- 表示消息在分区 log 文件中的「位置」。
**存储位置**：和消息数据一起写在 **分区 log 文件** 中。
**作用**：用来唯一标识消息在分区中的顺序位置
#### 消费offset
**定义**：消费者组（Consumer Group）维护的消费进度标记，表示自己「已经消费到哪条消息」
**含义**：指向「下一个要消费的消息」。例如：提交 offset=5 → 表示 0~4 已经处理，下次从 5 开始拉取。
**作用**：用来记录消费者组的消费进度，实现故障恢复、重平衡等功能。
**存储位置**：
    - Kafka 0.9.0 之前：存储在 Zookeeper。 
    - Kafka 0.9.0 之后：存储在 Kafka 内置主题 `__consumer_offsets`（默认 50 分区，可配置）
##### 消费 Offset 的提交
- **自动提交**：
    - `enable.auto.commit=true`（默认）。
    - Kafka 客户端会周期性（由 `auto.commit.interval.ms` 控制）把当前消费进度写入 `__consumer_offsets`。
- **手动提交**：
    - `enable.auto.commit=false`。
    - 程序需显式调用 `commitSync` 或 `commitAsync` 提交进度。
    - 优点：能精确控制「什么时候确认已消费」，适合严格的业务场景。
- **风险点**：提交失败或延迟 → 可能导致重复消费或少量消息丢失，需要应用层设计幂等或补偿机制。

##### 消费Offset的重置
- **手动重置**：
    - 调用 `seek(offset)`：从指定位置重新消费。
    - 调用 `seekToBeginning()`：从最早的消息开始。
    - 调用 `seekToEnd()`：跳到最新位置，只消费新写入的消息。
- **自动重置**（由 `auto.offset.reset` 参数控制）：
    - `earliest`：没有已提交 offset 时，从最早的消息开始。
    - `latest`：没有已提交 offset 时，从最新消息开始。
    - `none`：如果没有 offset，直接抛异常。

### 消费者组
多个消费者，只要指定了相同的 group_id ，即属于同一个消费者组，同一个消费者组内的消费者可以共同消费一个Topic中的数据。PS不同消费者组也可以共同消费同一个Topic中的数据。
##### 分配策略
*消费者是怎么分配同一个 topic 下的 Partition？*
- **RangeAssignor**（默认）
    - 按照 Partition 的序号区间平均分配。
    - 例：有 6 个 Partition，2 个 Consumer → C1 拿 P0~~P2，C2 拿 P3~~P5。
    - 特点：如果 Topic 多，可能分配不均衡。
- **RoundRobinAssignor**
    - 按轮询分配。
    - 例：6 个 Partition，2 个 Consumer → C1 拿 P0、P2、P4，C2 拿 P1、P3、P5。
    - 特点：更均衡，尤其是多 Topic 时。
- **StickyAssignor**
    - 在保证尽量均衡的前提下，尽量保持和上一次分配结果一致，减少 Rebalance 带来的数据重复消费。
    - Kafka 2.4+ 默认用它代替 Range。
- **CooperativeStickyAssignor**（Kafka 2.4+ 推荐）
    - 比 Sticky 更优雅，支持**渐进式 Rebalance**，不需要一次性把所有分配推翻，只迁移必要的 Partition。




## 消息积压
### 消费者端优化
1. 消费者**消费速度太慢**，导致消息积压，可以优化消费者代码逻辑
2. 当消费者数量少于partition分区数量时，可以**增加消费者数量**（但不能超过partition的数量）
3. 当手动提交offset时，**检查是否消费者未正确提交 offset**，导致消费停滞
### 生产者端优化
1. **控制生产速度**：例如在生产者端设置限流机制，避免消息生产速度过快
### Kafka集群优化
1. **调整分区数量**：根据消息生产和消费速度，合理调整主题的分区数量。如果消息堆积是由于分区数过少导致，可增加分区数。例如，将一个原本只有2个分区的主题，根据业务量增加到10个分区，以**提高并行处理能力**。但分区数过多也会增加管理开销，需谨慎评估。

