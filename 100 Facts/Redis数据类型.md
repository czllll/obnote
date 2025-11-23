
| domain | layer   | type  |
| ------ | ------- | ----- |
| #db    | #redis  | #fact |

## String
### 底层实现
* 使用的是 SDS（Simple Dynamic String）结构
#### Redis 3.2 之前结构
```c
struct sdshdr {
    unsigned int len;   // 记录 buf 数组中已使用字节的数量
    unsigned int free;  // 记录 buf 数组中未使用字节的数量
    char buf[];         // 字节数组，用于保存字符串
};
```
* `free`字段对于追加操作可以避免重新分配内存，对于缩短操作可以直接修改`len`和`free`大小，避免把内存空间还回去。
#### Redis3.2及以后结构
* 3.2 之前的头部包括`len`和`free`，即使存一个字符也需要这么长的头部，3.2 根据字符串长度范围采用不同的数据类型来记录 `len`和`alloc`，标志位`flags`即为 对应的数据类型
* 其中 `sdshdr5`比较特殊，处理长度<32的短字符串，长度放在了 `flags`里
 > 计算机最小寻址单位是“字节”不是“位”，因此不能将`flags`弄成3bit，而要弄成 1byte
```c
struct __attribute__ ((__packed__)) sdshdr5 {
    unsigned char flags; /* 低 5 位存储长度，高 3 位存储类型 */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr8 {
    uint8_t len;         /* 1 字节 */
    uint8_t alloc;       /* 1 字节 */
    unsigned char flags; /* 1 字节，记录类型 */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr16 {
    uint16_t len;        /* 2 字节 */
    uint16_t alloc;      /* 2 字节 */
    unsigned char flags; 
    char buf[];
};
/* 用于存储长度小于 4GB 的字符串 */
struct __attribute__ ((__packed__)) sdshdr32 {
    uint32_t len;        /* 4 字节：已使用长度 */
    uint32_t alloc;      /* 4 字节：总分配长度（不含头和结束符） */
    unsigned char flags; /* 1 字节：低 3 位存类型，高 5 位保留 */
    char buf[];          /* 柔性数组：数据内容 */
};

/* 用于存储超大字符串（理论上支持 64 位寻址空间） */
struct __attribute__ ((__packed__)) sdshdr64 {
    uint64_t len;        /* 8 字节：已使用长度 */
    uint64_t alloc;      /* 8 字节：总分配长度 */
    unsigned char flags; /* 1 字节：低 3 位存类型，高 5 位保留 */
    char buf[];          /* 柔性数组：数据内容 */
};
```

### 相较于 C 字符串的优势
* 二进制安全
* O(1)获得长度
* 动态扩容和安全
## List
* 有序集合，可以包含重复元素
### 底层实现
#### Redis 3.2 之前
* 阈值：元素数量超过 512|| 任意一个元素成员长度超过 64 字节
	* 超过后使用 Linkedlist
	* 没超过使用 Ziplist
#### Redis 3.2及之后
* 使用 Quicklist，
	* 结构：外层是一个双向链表，每个 Node 存一个 Ziplist/Listpack
## Set
* 无序集合，元素唯一
* 阈值：所有元素都是整数 && 集合中的元素数量**少于** `set-max-intset-entries`（默认 512 个）
	* 满足就是用 IntSet
		* IntSet 维护一个从小到大排序的整数数组，底层使用二分查找
	* 否则使用 Dict，其中 key 是 Set 的元素，value 位 null
## Hash
* 阈值：元素数量超过 512|| 任意一个 key 或 value 长度超过 64 字节，
	* 超过后使用 Dict，O(1)
	* 没超过使用 Listpack/Ziplist O(n)
### Dict
```c
typedef struct dictEntry {
    void *key;                // 键
    union {                   // 值 (使用联合体优化内存)
        void *val;            // 指向具体 RedisObject 的指针
        uint64_t u64;         // 纯无符号整数
        int64_t s64;          // 纯有符号整数
        double d;             // 浮点数
    } v;
    struct dictEntry *next;   // 指向下一个节点 (解决哈希冲突)
} dictEntry;
```
* 每一个 kv 对就是一个 dictEntry
* 当 value 是整数或者浮点数，redis 不会创建 `RedsObject`对象，而是直接放在结构体里
```c
typedef struct dictht {
    dictEntry **table;      // 哈希表数组 (Bucket 桶)
    unsigned long size;     // 数组的大小 (总是 2 的 n 次方)
    unsigned long sizemask; // 掩码 (总是 size - 1)，用于快速计算索引
    unsigned long used;     // 当前存了多少个节点
} dictht;
```
* `dictht`代表一个哈希表
* `table`每个元素都是一个指向 `dictEntry` 链表头的指针

```c
typedef struct dict {
    dictType *type;         // 类型特定函数 (怎么算哈希、怎么复制键...)
    void *privdata;         // 私有数据
    dictht ht[2];           // 【核心】两个哈希表：ht[0] 和 ht[1]
    long rehashidx;         // rehash 进度索引 (-1 表示未进行 rehash)
    unsigned long iterators; // 当前运行的迭代器数量
} dict;
```
* `ht[2]`就是包含两个 dictht 的数组
* **扩容**时的渐进式 rehash
	* **准备**：Redis 为 `ht[1]` 分配空间（大小通常是 `ht[0]` 的两倍）；将 `rehashidx`（搬迁进度索引）从 -1 改为 **0**。这标志着 Rehash 正式开始。
	* **被动搬迁**：用户对该 dict 执行 crud，redis 多做一步：将 `ht[0]` 中 `rehashidx` 索引位置上的**所有**键值对，迁移到 `ht[1]` 中，rehashidx++
	* **定时搬迁**：没有客户端会有定时后台线程来做。
	* **结束**：释放 ht0，ht0 = ht1，ht1 = null， rehashidx = -1
## ZSet
* 元素唯一，每个元素关联一个 score，按照 score 排序
* 阈值：Zset 元素 > 128 || Zset 种任意元素长度超过 64 字节
	* 超过时就改用 Skiplist+Dict
	* 没超过就使用Ziplist/Listpack
### Ziplist/Listpack
* 一块连续的内存空间，元素和分数排列在一起
* ``[ member1, score1, member2, score2, ... ]`` 分数从小到大排序
#### Ziplist
* 压缩列表，数据排在紧凑的一整块内存。
* **结构**
	* `zlbytes` 总字节数
	* `zltail` 最后一个元素距离起始位置的偏移量
	* `zllen` 元素个数
	* `entry` 实际存储的数据元素
		* `prevlen` 前一个节点的字节数 ，使用 1 个字节（能表示 最多254 字节，因为`zlend`是 0xff 被征用了， 0xfe 被 5 字节表示时的标志位征用了）或 5个字节（0xfe标志位和后续四个字节（能表示 4GB）表示长度）表示
		* `encoding ` 后面实际内容的类型
		* `entry-data` 实际内容
	* `zlend` 特殊标记表示结束 0xff
* **问题**
	* 插入/删除慢： 数据向后移动，开销大
	* 连锁更新：prevlen 在前面字节超过254KB时就不能用 1 个字节表示了， 需要使用 5 个字节，所以当前 entry 的大小会变大，可能连锁导致之后的 prevlen 变化 
#### Listpack
* 由于 Ziplist 连锁更新的缺点，在 Redis7.0，Listpack 全面取代 Ziplist
* **结构**
	* `Total Bytes` (4字节)：整个 Listpack 占用的总字节数
	- `Num Elements` (2字节)：元素的个数
	- `Entries`
		- `Encoding`
		- `Data`
		- `Backlen` : 记录当前节点的长度，用于从后向前遍历
			- 最高位 0 表示不用往前读了，1 表示前面还有
			- 低 7 位表示数据位
			- 128 就是 00000001 10000000
	- `End Byte` (1字节)：结束标记， `0xFF` 
### Skiplist+Dict
```c
typedef struct zset {
    dict *dict;      // 字典：维护 member -> score 的映射
    zskiplist *zsl;  // 跳表：维护 score 的排序和范围查找
} zset;
```
#### Skiplist
* **结构**
```c
typedef struct zskiplist {
    struct zskiplistNode *header, *tail; // 头指针，尾指针
    unsigned long length;                // 节点总数 (不含头节点)
    int level;                           // 当前跳表中最高的层数
} zskiplist;

/* 跳表节点 */
typedef struct zskiplistNode {
    sds ele;                    // 成员对象 (即 Member，例如 "user:1")
    double score;               // 分值 (用于排序，例如 100.0)
    struct zskiplistNode *backward; // 【后退指针】(指向前一个节点)
    
    /* 层级数组 (柔性数组) */
    struct zskiplistLevel {
        struct zskiplistNode *forward; // 【前进指针】
        unsigned long span;            // 【跨度】(这是 Redis 特有的优化)
    } level[];
} zskiplistNode;
```
* Redis的跳表在 L1 是双向链表， 在 L2 及以上是单向链表，backward 仅作用于 L1 层
* `span`是的 Redis 支持 O(logN)的 Rank 计算
* 如何确定层高？
	* 创建节点时，生成一个 `[0,1)` 范围的随机数；若该随机数 `< 0.25`（概率为 25%），则增加一层；然后继续生成下一个随机数，继续判断；直到某个随机数 ≥ 0.25 为止，最终确定该节点的层数。
##### 为什么使用跳表而不是红黑树/B+树？
>1) They are not very memory intensive. It's up to you basically. Changing parameters about the probability of a node to have a given number of levels will make then _less_ memory intensive than btrees.
>2)  A sorted set is often target of many ZRANGE or ZREVRANGE operations, that is, traversing the skip list as a linked list. With this operation the cache locality of skip lists is at least as good as with other kind of balanced trees.
>3)  They are simpler to implement, debug, and so forth. For instance thanks to the skip list simplicity I received a patch (already in Redis master) with augmented skip lists implementing ZRANK in O(log(N)). It required little changes to the code.
>[The Skip list | Hacker News](https://news.ycombinator.com/item?id=1171423)
* 代码简单易维护
* 范围查找
	* 红黑树需要找到起点以后再通过中序遍历找后续的
	* B+树可以范围查找，但是 B+树会浪费空间，因为他是固定分配页的
* 内存占用灵活
	* 跳表可以通过调整概率参数，灵活内存消耗和查询效率
## BitMap
* 底层就是 String，将 `char buf[]`看作是一连串的 Bit，
* `buf[0]` 对应第 0~7 位，`buf[1]` 对应第 8~15 位
## HyperLog
## GEO
## Stream

