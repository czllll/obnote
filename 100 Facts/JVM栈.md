
| domain | layer | type  |
| ------ | ----- | ----- |
| #java  | #jvm  | #fact |

即Java虚拟机栈，属于JVM内存结构的一部分，是线程私有的。
## 作用原理
每个线程有自己的 **虚拟机栈**（由 `-Xss` 控制大小），每次方法调用时，JVM 会在虚拟机栈里压入一个 栈帧（Stack Frame）方法执行完，栈帧就会出栈。
## 栈帧
栈帧是方法执行的基本单位
### 栈帧结构
* 局部变量表
* 操作数栈
* 动态链接
* 返回地址
* 附加信息
	* Lock Record
		* 当方法里用了 `synchronized`，在栈帧里会额外分配一块区域保存 锁记录
		* 用来支持轻量级锁：对象头（Mark Word）(见 [[JVM 堆]])会指向栈里的这块记录
	* JIT 编译器需要的调试信息
使用结构体模拟如下：
```java
class Frame {
    LocalVariableTable localVars;   // 局部变量表
    OperandStack operandStack;      // 操作数栈, 执行字节码时 push/pop
    ConstantPoolRef constantPool;   // 动态链接引用
    int returnAddress;              // 返回地址（调用方的字节码位置）
    LockRecord[] lockRecords;       // (HotSpot 扩展) synchronized 用,用来记录轻量级锁信息
    // 其他调试 / JIT 附加信息
}
```
## Lock Record
### 结构
```ascii
displaced_header = 原始MarkWord (含hash?) 
obj_ref = 指向 obj   
```
