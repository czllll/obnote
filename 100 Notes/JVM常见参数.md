#jvm 
# JVM 常见参数总结

## 1. 堆内存相关

|参数|作用|单位|常见设置|
|---|---|---|---|
|`-Xms<size>`|堆初始大小|k/m/g|一般与 `-Xmx` 相等，例如 `-Xms2g`|
|`-Xmx<size>`|堆最大大小|k/m/g|常设为机器物理内存的 1/2 ~ 2/3|
|`-Xmn<size>`|新生代大小|k/m/g|一般为堆大小的 1/3 ~ 1/4|
|`-XX:NewRatio=n`|老年代:新生代 比例|整数|默认 2，表示 1:2|
|`-XX:SurvivorRatio=n`|Eden:Survivor 比例|整数|默认 8，表示 8:1:1|

## 2. 元空间 / 永久代

|参数|作用|单位|备注|
|---|---|---|---|
|`-XX:PermSize=size`|永久代初始大小|k/m/g|JDK7 及以前|
|`-XX:MaxPermSize=size`|永久代最大大小|k/m/g|JDK7 及以前|
|`-XX:MetaspaceSize=size`|元空间初始大小|k/m/g|JDK8+|
|`-XX:MaxMetaspaceSize=size`|元空间最大大小|k/m/g|JDK8+|

## 3. 垃圾回收器（GC）

|参数|含义|
|---|---|
|`-XX:+UseSerialGC`|单线程 GC，适合小堆|
|`-XX:+UseParallelGC`|多线程 GC（吞吐量优先）|
|`-XX:+UseConcMarkSweepGC`|CMS 低延迟 GC（JDK14 已移除）|
|`-XX:+UseG1GC`|G1 GC（JDK9+ 默认）|
|`-XX:+UseZGC`|ZGC，超低延迟 GC（JDK11+ 可用）|
|`-XX:+UseShenandoahGC`|Shenandoah GC，低延迟（RedHat 提供）|

常见调优参数：
- `-XX:MaxGCPauseMillis=n` → 期望的最大停顿时间（毫秒）
- `-XX:GCTimeRatio=n` → 吞吐量目标（1/(1+n)）
## 4. 线程与栈

| 参数           | 作用              | 单位  | 常见设置                      |
| ------------ | --------------- | --- | ------------------------- |
| `-Xss<size>` | 每个线程[[JVM栈]]大小 | k/m | 默认 1M，常调小到 256k~512k（省内存） |

## 5. 调试 & 日志

|参数|作用|
|---|---|
|`-XX:+PrintGC`|打印 GC 简要日志|
|`-XX:+PrintGCDetails`|打印 GC 详细日志|
|`-XX:+PrintGCDateStamps`|GC 日志加时间戳|
|`-Xloggc:<file>`|将 GC 日志写入文件|
|`-XX:+HeapDumpOnOutOfMemoryError`|OOM 时生成堆转储|
|`-XX:HeapDumpPath=<file>`|指定堆转储文件保存路径|

## 6. 诊断 & 调优

|参数|作用|
|---|---|
|`-XX:+PrintFlagsFinal`|打印所有参数及最终值|
|`-XX:+PrintCommandLineFlags`|打印命令行参数|
|`-XX:+UnlockExperimentalVMOptions`|解锁实验性参数|
|`-XX:+UnlockDiagnosticVMOptions`|解锁诊断参数|

## 7. 常见组合示例

生产环境经常这样配：

```bash
java -Xms2g -Xmx2g -Xmn512m -Xss512k \
     -XX:+UseG1GC -XX:MaxGCPauseMillis=200 \
     -XX:+PrintGCDetails -XX:+PrintGCDateStamps \
     -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/var/log/heapdump.hprof \
     -Xloggc:/var/log/gc.log -jar app.jar
```

---

👉 **单位总结**：

- `k` / `K` = KB
    
- `m` / `M` = MB
    
- `g` / `G` = GB  
    （不写单位时按字节算，几乎没人这么用）
    

---

要不要我再帮你整理一份 **不同场景的推荐参数配置**（比如开发机 / 测试机 / 生产大堆）？这样你能直接套用。