#bagu 

### 数组与集合的区别？

* 数组
  * 固定长度的数据结构
  * 可以包含基本数据类型和对象
  * 可以直接访问元素
* 集合
  * 动态长度的数据结构
  * 只能包含对象
  * 需要通过迭代器或其public 方法 如get()访问

### 说说Java中的集合

![image-20250206141211810](https://img.dirtsai.work/astro-blog/2025/02/2f2abbd5e24d3ea46ca48b3fae0281a4.png)

* List

  * 有序的Collection，该接口能精确控制每个元素的插入位置，用户能根据索引访问List中的元素
  * LinedList（非线程安全）
    * 双向链表
    * 插入和删除速度快、随机访问速度更慢

  * ArrayList（非线程安全）
	  * 底层：
		  * 一个可变长度的数组（`Object[]`）
		  * transient Object[] elementData; // 真正存数据的地方
		  * private int size;               // 实际存了多少个元素
    * 容量可变的非线程安全列表，底层使用数组实现
    * 集合扩容时会创建更大的数组（1.5倍），并把原数组复制到新数组
    * 支持对元素的快速随机访问，插入和删除可能需要移动大量元素，所以可能速度很慢
  * Vector
  * Stack

* Set

  * Set不允许存在重复的元素，与List不同set中的元素是无序的
  * HashSet
    * 通过HashMap实现，HashMap的Key极为HashSet存储的元素，所有Key均使用相同的Value和一个名为PRESENT的Object类型常量
    * 线程不安全
  * LinkedHashSet
    * 继承自HashSet通过LinkedHashMap实现，使用双向链表维护元素插入顺序
  * TreeSet
    * 通过TreeMap实现
    * 添加元素到集合时按照比较规则将其插入合适的位置，保证插入后的集合仍然有序

* Map

  * Key无序，唯一
  * Value不要求有序，允许重复
  * Map没有继承于Collection接口
  * HashMap
    * JDK1.8之前由数组和链表组成，链表为解决哈希冲突而存在
    * JDK1.8之后，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间；数量小于6时，将红黑树转化成链表
  * LinkedHashMap
    * 继承自HashMap
    * 增加了一条双向链表，使得上买呢结构可以保持kv的插入顺序
  * HashTable
    * 数组和链表组成，链表为解决哈希冲突而存在
  * TreeMap
    * 红黑树--自平衡的排序二叉树
  * ConcurrentHashMap
    * Node数组+链表+红河书实现
    * 线程安全
      * JDK1.8之前Segment锁
      * JDK1.8之后volatile + CAS或者synchronized

### Java线程安全的集合是什么

* Java.util包中的线程安全的类主要有2个，其他都是非线程安全类
* Vector
  * 线程安全的动态数组
  * 内部方法基本都通过synchronized修饰
  * 内部使用Object[]数组保存数据，默认容量为10，size达到capacity时，默认扩容为2倍，创建新数组，复制原数组的内容
* HashTable
  * 线程安全的哈希表
  * 每个方法加上synchronized关键字，锁住整个table对象，不支持null键和值

* Java.util.concurrent包提供的都是线程安全的集合
  * 并发Map
    * **CurrentHashMap**
      * 1.8之前，加的是分段（Segment）锁，修改时才需要获得锁，读操作不需要加锁
        * Segment是一种[[ReentrantLock锁]]
        * 一个ConcurrentHashMap里包含一个Segment数组（Segment结构和HashMap类似）
        * 一个Segment包含一个HashEntry数组
          * 每个HashEntry是一个链表结构
      * 1.8及之后，取消segment字段，改用Node数组+链表/红黑树，hash值相同的元素会链在同一个Node下形成链表或红黑树
        * 对于put操作，
        * 如果key对应的数组元素为null，通过[[CAS操作]]将其设置为当前值；
        * 若Key对应的数组元素不为null（冲突时）
          * 使用synchronized锁住当前Node首节点
          * 遍历链表寻找：找到更新，未找到添加到链表尾部
          * 链表转红黑树
            * 单个Node下的链表长度超过8
            * 主体Node[]数组长度达到64
            * 同时满足
          * 释放synchronized锁
      * 读操作无锁，写操作仅锁定必要的部分，而不是整个Map
    * ConcurrentSkipListMap
      * 实现可一个基于SkipList算法的可排序的并发集合
      * 可以在O(logn)预期时间内完成搜索，插入，删除等操作的数据结构，通过维护多个指向其他元素的“跳跃”连接来实现高效查找
  * 并发List
    * CopyOnWriteArrayList
      * 是ArrayList的线程安全的变体
      * 当对象写时，使用Lock做同步处理，内部拷贝原数组，并在新数组上添加操作，最后将新数组替换掉旧数据；
      * 若进行的读操作，则直接返回结果，操作过程中不需要进行同步
  * 并发Set
    * ConcurrentSkipListSet
      * 线程安全的有序集合，底层使用ConcurrentSkipListMap实现
    * CopyOnWriteArraySet
      * 线程安全的无序集合
      * 通过CopyOnWriteArrayList实现的
  * 并发Queue
    * ConcurrentLinkedQueue
      * CAS实现高并发
    * BlockingQUeue
      * 主要功能不在于提升高并发的队列性能，而在于简化多线程间的数据共享
      * 提供读写阻塞等待的机制，生产者/消费者
  * 并发Deque
    * LinkedBlockingDeque
      * 未进行读写锁的分离，同一时间只能有一个线程对其操作
    * ConcurrentLinkedDeque
      * 双向链表结构
      * 使用CAS操作保证并发安全
      * 无锁设计，所有字段用volatile修饰
      * 投喂节点支持双向遍历和操作



### 集合遍历的方法

* for循环
* for-each
* Iterator迭代器
* ListIterator迭代器
  * 是Iterator的子类多了双向遍历和修改

```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

ListIterator<String> iter = list.listIterator();

// 向后遍历
while(iter.hasNext()) {
    System.out.println(iter.next());
}

// 向前遍历
while(iter.hasPrevious()) {
    System.out.println(iter.previous());
}

// 修改操作
iter = list.listIterator();
while(iter.hasNext()) {
    String item = iter.next();
    if(item.equals("B")) {
        iter.set("修改B");  // 修改当前元素
        iter.add("在B后添加"); // 在当前位置后添加
    }
}

```

* forEach方法
  * `list.forEach(e -> System.out.println(e));`
* Stream API
  * `list.stream().forEach(e -> System.out.println(e));`



### list可以一边遍历一边修改元素吗 

* 使用for循环
  * 可以
* for-each循环
  * 不建议
  * for-each底层基于迭代器实现，在遍历过程中修改集合结构会导致迭代器的语气结构和实际结构不一致，可能会引发异常
* 使用迭代器遍历
  * 可以用迭代器的remove删除
  * 修改也需使用迭代器的set，若使用list的set方法也会抛出异常



### list如何快速删除某个指定下标的元素

* ArrayList使用remove()，底层是数组结构，平均时间复杂度为O(n)
* LinkedList使用remove()，底层是链表结构，平均时间复杂度为O(n)

* copyOnWriteArrayList使用remove
  * 获取锁，创建长为n-1的数组，分两段复制到新数组
  * 用新数组替换原数组引用，释放锁



### ArrayList和LinkedList区别，哪个集合是线程安全的？

* 底层数据结构不同
  * ArrayList数组
  * LinkedList链表
* 插入删除操作的效率不同
  * Arraylist需要移动元素
  * LinkedList调整指针即可
* 随机访问效率不同
  * ArrayList支持通过索引快速随机访问
  * LinkedList需要遍历
* 空间
  * ArrayList在创建时需要分配一段连续的内存空间
  * LinkedList每个节点只需要存储元素和指针
* 线程安全
  * 两者都不，vector是线程安全的

### 把ArrayList变成线程安全的方法有哪些

* 使用Collections类的synchronizedList方法将ArrayList包装成线程安全的List
  * 通过[[装饰器模式]]，创建一个代理类包装ArrayList，这个代理类所有方法都是用synchronized(mutex)代码块来同步访问原始ArrrayList操作
  * 封装原始的ArrayList，所有操作都通过synchronized代码块同步
  * 迭代器方法需要外部手动同步
* 使用CopyOnWriteArrayList替代ArrayList
* 使用Vector代替ArrayList

### 为什么ArrayList不是线程安全的？

* 数据一致性问题
  * 多个线程同时操作时，可能一个线程的操作会覆盖另一个线程的操作
* 扩容时的线程安全问题
  * 当需要扩容时，会创建新数组并把原数组数据复制过去，若此时有其他线程正在rw操作，可能触发数组越界异常
    * 由于在复制完成前，引用仍然指向旧数组

### ArrayList的扩容机制？

* 计算新容量
  * 新容量会扩大为原容量的1.5倍，然后检查是否超过了最大容量限制
  * JDK10之后调整了扩容策略
* 创建新数组
* 复制元素
* 更新引用：将ArrayList内部指向原数组的引用指向新数组
* 完成扩容

### CopyonWriteArraylist如何实现线程安全的？

* CopyonWriteArraylist底层也是通过一个数组保存数据，使用volatile关键词修饰数组，保证当前线程对数组对象重新赋值后，其他线程可以及时感知到
* 在写入时，使用互斥锁ReentranLock保证线程安全，修改操作时候，复制整个数组
* 读操作无锁

### Map

### Hashmap的结构

```java
public class HashMap<K,V> {
    // 核心数组，称为哈希表
    transient Node<K,V>[] table;

    // Node是HashMap的一个内部类，表示一个键值对节点
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;  // key的哈希值
        final K key;     // 键
        V value;         // 值
        Node<K,V> next;  // 下一个节点的引用

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
    }

    // 实际存储的键值对数量
    transient int size;

    // 负载因子，默认0.75f
    final float loadFactor;

    // 扩容阈值 = 容量 * 负载因子
    int threshold;
}
```



* table: Node<K,V>[] 数组， K，V为泛型

* Node节点：存储每一个KV对的实体类

  * hash：整数类型，存储key的哈希值
  * key：键
  * value：值
  * next：指向下一个节点的引用，类型也是Node<K,V>
    * 用于形成链表结构，解决哈希冲突

* 其他成员变量

  * size：记录当前hashmap中实际存储的键值对数量
  * loadFactor：负载因子，控制数组扩容的时机（0.75）
  * threshold：扩容阈值，当szze超过这个值时会触发扩容

  

### 如何对map进行快速遍历？

* 使用for-each循环和entrySet()方法，同时获取kv
* 使用for-each循环和keySet()方法，获取key
* 使用迭代器：
  * 获取map的entrySet或keySet的迭代器
  * 在需要删除元素操作时比较有用
  * 使用迭代器的remove()方法会同步更新expectedModCount，不会抛出异常

> 当使用 for-each 循环删除元素时，底层实际是用 Iterator 实现的。每次调用 map.remove() 会修改集合的结构，而 Iterator 内部有一个 modCount 用来记录集合的修改次数。当发现 modCount 和 expectedModCount 不一致时，说明集合在遍历过程中被修改，就会抛出 ConcurrentModificationException。

* 使用Lambda表达式和forEach()方法
  * `map.forEach((k,v) -> System.out.println(k+v));`
* 使用Stream API，将Map转换为流再进行操作
  * `map.entrySet().stream().forEach(entry -> System.out.println(enrty.getKey));`

### Hash冲突解决方式？

* 链接法
  * 使用链表或其他数据结构来存储冲突的kv对，链接在同一个hash桶中
* 开放寻址法：在hash表中找到另一个可用的位置
  * 现行探测
  * 二次探测
  * 双重散列
* rehash
  * 使用另一个hash函数再次计算键的hash值
* hash扩容
  * 动态扩大哈希桶的的数量，重新分配kv对



### 如何使得hashmap线程安全？

* Collections.synchronizedMap同步加锁
* 使用ConcurrentHashmap



### 介绍一下hashmap的put过程

* 首次插入
  * 检查table是否为空或者为null
  * 如果是，初始化默认容量16的数组
  * 计算扩容阈值：16*0.75 = 12
* hash值计算
  * 获取key的hashCode
  * 将hashCode的高16位与低16位异或，增加随机性
    * 这是由于hashMap的数组长度一般比较小，使用(n-1)&hash计算时只有hash值的低位参与运算；这样可以使高位也参与运算
  * 根据hash值和数组长度计算索引：（n-1）& hash
* 数组位置为空的情况
  * 直接创建新节点放入
  * modCount + 1（modCount用来统计HashMap发生结构性变化的次数）
  * size + 1
  * 检查是否需要扩容
* 数组位置不为空（发生hash冲突）
  * 先检查第一个节点：
    * 比较hash值是否相等
    * 比较key值是否相等（==或equals），相等则更新value
  * 如果是红黑树节点
    * 按照红黑树方式插入或更新节点
    * 按照key的hash值和equals判断
  * 如果是链表节点
    * 遍历链表查找相同的key，找到则更新；没找到则插入链表尾部
    * 插入后检查链表长度是否达到8
      * 若达到8且table数组长度>=64，转红黑树
      * 若达到8但table数组长度<64,调用resize()扩容
* 扩容检查
  * 元素个数超过阈值时扩容
  * 新容量和新阈值均为原来的2倍
  * 重新计算所有元素的位置，重新放置元素
    * 链表根据hash&oldCap拆分链表，可以使得节点在扩容后是留在原位置还是移动到原位置+oldCap的位置



### HashMap的get过程

* 计算数组索引
  * 获取key的hashCode
  * 将hashCode的高16位与低16位异或得到hash值
  * 用(n - 1) & hash 计算数组索引
* 检查第一个节点
  * 比较hash值是否相等
  * 若hash相等，再用equals比较key
  * 如果都相等，返回该节点的值
* 处理冲突
  * 若是红黑树，调用getTreeNode在树中查找
  * 若是链表，遍历链表。
    * 对每个节点都进行hash和equals的比较
    * 找到则返回，找不到就返回null



### hashmap调用get方法一定安全吗

* hashmap未初始化情况下，使用get会发生空指针异常
* 线程安全，同时一个线程读，一个线程写，就会发生同步异常

### hashmap一般用什么做key？为什么String适合做key？

* 用string做key，因为string对象是不可变的，一旦创建就不能修改，保证了key的稳定性
* 若key是可变的，可能会导致hashcode和equals方法的不一致，进而影响hashmap的正确性

### 为什么hashmap要用红黑树而不是平衡二叉树？

* 平衡二叉树追求的是完全平衡的状态，任何节点的左右子树高度差不会超过1，每次插入或删除时都需要进行左旋或者右旋来调整树的结构
* 红黑树追求的是弱平衡的状态。整个树最长路径不会超过最短路径的2倍。这使得红黑树在插入和删除等操作过程中，不会频繁破坏红黑树规则而进行频繁调整。

### hashmap key可以为null吗？

* 可以
* hashmap使用hash()方法来计算key的哈希值，当key为空时，直接会使得key的哈希值为0，不走key.hashCode()方法



### Hashmap中的hashcode方法是什么？

* hashCode
  * 用于计算对象的hash值，决定对象子啊HashMap中的存储位置
  * String的hashCode
    * 使用延迟计算+缓存机制
    * 计算公式：s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]

### 重写Hashmap的equals方法时为什么要同时重写hashCode方法呢？

* 因为需要保证若两个对象equals比较是相等的，那么它们的hashCode值也必须相等
* 当使用equals方法重新定义了相等规则时，也需要使用hashCode使用相同的规则来生成hashCode

### HashMap在多线程下可能会出现的问题

* JDK1.7中的Hashmap使用头插法，多个线程同时扩容会出现环形链表情况
* 多线程同时执行put操作，计算出来索引位置相同，则会被覆盖

### Hashmap的扩容机制介绍一下

* hashmap默认的负载因子是0.75，若hashmap中的元素个数超过总容量的75%，则会触发扩容
  * 哈希表长度扩展为原来两杯
  * 旧哈希表放在心哈希表中
* 由于使用2次幂的扩展，元素的位置要么在原位置，要没在原位置再移动二次幂的位置
  * 即hash值新增的bit，0的话就在原位置；1的话就在原索引+oldCap的位置



### HashMAp大小为什么是2的n次方

* 在JDK1.7中，扩容采用的是头插法，扩容时会从原始链表的尾部开始遍历，没遍历一个节点就将其插入到新数组对应位置的链表头部。这导致原链表中的元素在新链表中是反转的
* JDK1.8采用尾插法，通过与运算来重新分配索引



### ConcurrentHashMap JDK1.8 如何实现线程安全的

* concurrentHashMap通过对头节点加锁来保证线程安全
* 添加元素时
  * 判断容器是否为空
    * 为空就使用volatile+CAS初始化表
  * 容器不为空，计算元素位置
  * 根据计算位置情况三种处理
    * 目标位置为空
      * CAS直接尝试插入：CAS能保证只有一个线程成功插入
    * 目标位置不为空且无哈希冲突
      * 更新value
    * 有哈希冲突时
      * 使用synchronized锁住当前的桶
      * 判断是链表还是i红黑树结构并遍历结构插入节点
      * 若是链表则判断是否需要树化
      * 完成后释放锁



### CAS是什么

* CAS（COmpare And Swap）是一种无锁算法
* 三个操作数
  * 内存位置V
  * 预期原值A
  * 新值B
* 执行过程
  * 比较内存位置V的值与预期A值是否相同
  * 如果相同，将内容位置的值更新为新值B
  * 如果不同，说明其他线程修改过，操作失败，通常会重试
* 缺点
  * ABA问题
    * 值从A变为B又变回A，CAS会觉得值没变
    * 使用版本号解决

### 乐观锁和悲观锁是什么

* 乐观锁
  * 认为并发冲突不会发生
  * 不加锁，在更新时检查是否有冲突
* 悲观锁
  * 认为并发冲突经常发生
  * 先加锁，后操作

## Set

### Set集合有什么特点？如何实现key无重复的

* 特点：
  * 元素为宜，不会出现重复元素
* 实现原理
  * 内部数据结构来实现。当插入时，根据hashCode来确定元素的存储位置，然后再通过equals方法来判断是否云在相同的元素，如果存在则不会再次插入。

### 有序Set是什么？记录插入顺序的集合是什么？

* 有序的Set是TreeSet和LinkedHashSet
  * TreeSet基于红黑树
  * LinkedHashSet基于双重链表和哈希表的结合来实现元素的有序存储
* 记录插入顺序的集合LinkedHashSet
  * 保证插入顺序

