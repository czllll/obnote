---
tags:
- java
- juc
---
* 定位：底层工具类， 提供直接操作内存，内存屏障，CAS ，线程调度的能力
* 特点：JUC 很多都依赖 Unsafe，如`AtomicXxx`、`AQS`、`LockSupport`
* Unsafe类为一[[单例模式]]实现，提供静态方法getUnsafe获取Unsafe实例，当且仅当调用getUnsafe方法的类为*引导[[类加载器]]BootstrapClassLoader*所加载时才合法，否则抛出SecurityException异常
	* 为什么呢？限制最顶层的类加载器来使用，效果就是只有 JDK 核心类库（`java.*`、`sun.*` 这些随 JDK 自带、由 Bootstrap 加载的类）才允许直接拿到 `Unsafe` 单例
## 源码
```java
// JDK8
public final class Unsafe {

    // 单例对象
    private static final Unsafe theUnsafe;

    private Unsafe() {
    }

    @CallerSensitive
    // @CallerSensitive 这个注解高告诉 JVM 真实调用者，来判断是否是BootstrapClassLoader加载的
    public static Unsafe getUnsafe() {
        Class var0 = Reflection.getCallerClass();

        // 仅在引导类加载器 `BootstrapClassLoader` 加载时才合法
        if (!VM.isSystemDomainLoader(var0.getClassLoader())) {
            throw new SecurityException("Unsafe");
        } else {
            return theUnsafe;
        }
    }
}



```
* 在 JDK8时，虽然我们不能直接通过反射使用类的 getUnsafe()方法来获取 unSafe 实例，但是可以通过反射获取 unSafe 单例字段，从而绕过@CallerSensitive的Reflection.getCallerClass();类加载器的检查来获取 unSafe 单例。拿到 unSafe 单例我们可以进行操作，比如 defineClass，但是这个方法里面也要检查Reflection.getCallerClass();，而此时我们可以通过`MethodHandle`这样的伪装来绕过这个限制，因此不够安全。
* 在 JDK9+中， @CallerSensitive也是一个标记，我们丢弃了Reflection.getCallerClass()，即 我们修改了JVM 信息提供方。使用了`StackWalker`这样的栈遍历器，更加安全和高效。
* JVM 使用本地方法提供信息，Reflection.getCallerClass();。JDK 9+ 对 `Unsafe` 进行了**“拆分与委托 (split and delegate)”。它将实现**拆分**到了内部的 `jdk.internal.misc.Unsafe`，而保留的 `sun.misc.Unsafe` 则将调用委托给前者，JIT 直接替换成了 cpp 的新逻辑。
```java
// 文件位置: src/java.base/share/classes/jdk/internal/misc/Unsafe.java

package jdk.internal.misc;

import sun.reflect.annotation.CallerSensitive;
import sun.reflect.annotation.ForceInline;

public final class Unsafe {

    private static final Unsafe theUnsafe = new Unsafe();

    private Unsafe() {}

    @CallerSensitive
    @ForceInline
    public static Unsafe getUnsafe() {
        return theUnsafe;
    }
}

package sun.misc; // 这是“包装类”所在的包

// 关键的 import 语句！它导入了真正的“工人”Unsafe 类
import jdk.internal.misc.Unsafe;

public final class Unsafe { // 这个类是 sun.misc.Unsafe，即“包装工”

    // 1. 获取那个真正的、内部的 Unsafe 实例，并保存起来。
    //    这个调用是明确指向 jdk.internal.misc.Unsafe.getUnsafe() 的。
    //    这个调用也是被 JVM 特殊处理的、真正执行安全检查的地方。
    //    我们给它起个明确的名字，叫 internalUnsafe。
    private static final jdk.internal.misc.Unsafe internalUnsafe = jdk.internal.misc.Unsafe.getUnsafe();

    // 2. “包装工”本身也有一个自己的单例实例。
    private static final Unsafe theUnsafe = new Unsafe();

    // 3. 构造函数是私有的。
    private Unsafe() {}

    // 4. 当外部调用 getUnsafe() 时，它执行自己的检查，并返回“包装工”的实例。
    @CallerSensitive
    public static Unsafe getUnsafe() {
        Class<?> caller = Reflection.getCallerClass();
        if (caller.getClassLoader() != null)
            throw new SecurityException("Unsafe");
        return theUnsafe; // 注意：返回的是 sun.misc.Unsafe 的实例
    }

    // 5. 所有危险的方法，都只是一个“传话筒”，把调用转发给内部的“工人”。
    public long objectFieldOffset(Field f) {
        // 调用被转发给了 internalUnsafe 实例去执行
        return internalUnsafe.objectFieldOffset(f);
    }

    public void putInt(Object o, long offset, int x) {
        // 调用被转发给了 internalUnsafe 实例去执行
        internalUnsafe.putInt(o, offset, x);
    }
    
    // ... 所有其他 Unsafe 方法都遵循这个模式 ...
}

```
## 功能
* 内存操作、CAS、Class相关、对象操作、线程调度、系统信息获取、内存屏障、数组操作
### 内存操作
* 一般 Java 对象都被分配到堆上，JVM 有 GC 来管理回收；Java 对于堆外内存的操作就通过 UnSafe 类进行。
* 为什么使用堆外内存？
	* 用户可通过 JNI 使得 Java 对象不分配在堆内而是堆外，在堆内就留一个内存地址。所以堆内内存规模小，GC的 STW 就小，性能就好
	* IO 会从堆内到堆外拷贝， 生命周期较短的对象，建议分配到堆外。
### CAS
* CAS 实际上就是通过 unSafe 实现的。Unsafe提供的CAS方法（如compareAndSwapXXX）底层实现即为CPU指令cmpxchg
### 线程调度
* park：线程将一直阻塞直到超时或中断等条件出现
* unpark：终止一个挂起的线程，使其恢复正常
	* 应用：AQS 的`LockSupport.park()`和`LockSupport.unpark()`，在 hotspot 下实际调用就是`Unsafe.park()`和`Unsafe.unpark()`

### 对象操作
* Unsafe中提供allocateInstance方法，仅通过Class对象就可以创建此类的实例对象，而且不需要调用其构造函数、初始化代码、JVM安全检查等
* 应用：
	* Gson 反序列化，用 `ConstructorConstructor` 来决定如何生成对象
		- **有无参构造函数** → 用反射 `Constructor.newInstance()`。
		- **接口/抽象类**（如 `List`, `Map`） → 返回默认实现（`ArrayList`, `HashMap`）。
		- **没有无参构造器，或者构造器不可访问** → 使用 `UnsafeAllocator.allocateInstance()`，即通过 `Unsafe.allocateInstance()` 直接分配对象（绕过构造函数）。
### 内存屏障
* `Unsafe` 提供的内存屏障方法是直接操作的原语
### 数组相关
* arrayBaseOffset与arrayIndexScale在AtomicIntegerArray数组每个元素的原子性操作会用到