#bagu 

## 概念

### 数组与集合的区别？

* 数组
  * 固定长度的数据结构
  * 可以包含基本数据类型和对象
  * 可以直接访问元素
* 集合
  * 动态长度的数据结构
  * 只能包含对象
  * 需要通过迭代器或其他方法访问

### 说说Java中的集合

![image-20250206141211810](https://img.dirtsai.work/astro-blog/2025/02/2f2abbd5e24d3ea46ca48b3fae0281a4.png)

* List

  * 有序的Collection，该接口能精确控制每个元素的插入位置，用户能根据索引访问List中的元素
  * LinedList（非线程安全）
    * 双向链表
    * 插入和删除速度快、随机访问速度更慢

  * ArrayList（非线程安全）
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

* 根据要添加的键的hashcode计算在数组中的位置（索引）
* 检查该位置是否为空
  * 若为空，在该位置创建一个新的Entry对象来存kv
  * 将HashMap的修改次数（modCount） 加1，以便在进行迭代时发现并发修改
* 若该位置不为空，检查该位置的第一对kv的hashcode是否与要添加的相同
  * 若相同，则表示找到了相同的key，直接替换完成更新
* 若不同，则遍历链表或红黑树来查找是否有相同的键
  * 若Entry集合是链表结构
    * 从链表的头部开始逐个比较键的hashcode和equals()方法，
      * 找到相同的键则更新
      * 没找到，则将新的键值添加到链表头部
  * 若Entry集合是红黑树结构，在红黑树使用hashcode和equeals()方法进行查找
    * 找到相同的键就更新
    * 没找到，就添加新的键值对到红黑树中
* 检查链表长度是否达到阈值（默认8）
  * 若长度超过阈值，且HashMap的数组长度大于等于64，则会将链表转换为红黑树，以提高查询效率
* 检查负载因子是否超过阈值（默认0.75）
  * 若键值对的数量（size）与数组的长度的比值大于阈值，则需要扩容
* 扩容
  * 创建一个新的两倍大小的数组
  * 将旧数组中的键值对重新计算哈希码并分配到新数组中的位置
  * 更新HashMap的数组引用和阈值参数、
* 完成添加操作



### HashMap的put和get过程

* 存储对象时，我们将KV传给put方法时，它调用hashCode计算hash从而得到bucket位置，进一步存储。HashMap会根据当前bucket的占用情况自动调整容量
* 获取对象时，get方法调用hashcode计算hash从而得到bucket位置，并进一步调用equals()方法确定kv对，如果发生碰撞，Hashmap通过链表将产生冲突的元素组织起来，jdk8及以上，若bucket中碰撞冲突元素超过某个限制（8），使用红黑树替换链表，从而提高速度。

