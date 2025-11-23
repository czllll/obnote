
| domain | layer | type  |
| ------ | ----- | ----- |
| #db    | #     | #fact |

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
## Set
## Hash
## ZSet
* 元素唯一，每个元素关联一个 score，按照 score 排序
### 底层实现（按占用空间分为两种）
#### Ziplist/Listpack
* 一块连续的内存空间，元素和分数排列在一起
* ``[ member1, score1, member2, score2, ... ]`` 分数从小到大排序
##### Ziplist
* 压缩列表，数据排在紧凑的一整块内存。
* **结构**
	* zlbytes 总字节数
	* zltail 最后一个元素距离起始位置的偏移量
	* zllen 元素个数
	* entry 实际存储的数据元素
		* prevlen
		* encoding
		* entry-data
	* zlend 特殊标记表示结束

#### Skiplist+Dict
* 阈值？
```c
typedef struct zset {
    dict *dict;      // 字典：维护 member -> score 的映射
    zskiplist *zsl;  // 跳表：维护 score 的排序和范围查找
} zset;
```
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
## HyperLog
## GEO
## Stream

