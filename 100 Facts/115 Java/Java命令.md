---
tags:
- java
- javase
---
### 1. 启动单个类
```bash
javac HelloWorld.java      # 编译生成 HelloWorld.class
java HelloWorld            # 启动程序，执行 main()
```
### 2. 启动普通 jar 包
#### 没有 Main-Class
```bash
java -cp myapp.jar com.example.Main
```
- `-cp` 指定 classpath，告诉 JVM 去哪里找类。
- 必须手动指定入口类（`com.example.Main`）。
#### 有 Main-Class（写在 MANIFEST.MF 里）
```bash
java -jar myapp.jar
```
- 直接用 `-jar`，JVM 会自动读取 jar 包里的 `Main-Class` 作为入口点。
### 3. 启动 Spring Boot 应用
Spring Boot 打包出来就是一个 **可执行 jar**，内部已经带了 `Main-Class`（即 `@SpringBootApplication` 类）。
标准启动命令：
```bash
java -jar app.jar
```
加上 [JVM 参数](JVM调优.md)：
```bash
java -Xms256m -Xmx512m -XX:+UseG1GC -jar app.jar
```
- `-Xms`：初始堆大小
- `-Xmx`：最大堆大小
- `-XX:+UseG1GC`：使用 G1 收集器
- 这些参数常用 `JAVA_OPTS` 环境变量传入，方便在容器中覆盖。
