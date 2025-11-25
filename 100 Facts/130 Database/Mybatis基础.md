---
tags:
- db java
---
source：[MyBatis技术内幕](https://weread.qq.com/web/reader/e9132aa0813ab7c34g0113edk45c322601945c48cce2e120)
Mybatis是一个Java持久层框架，可以帮助程序员完成[[ORM映射]]、查询缓存等常用功能。Mybatis 本质上是对 [[JDBC基础]]的封装和优化
# 为什么需要？
使用JDBC过于简单粗暴（直接访问数据库，直接关闭数据库连接）。JDBC的ResultSet到JavaBean的转化不能很好的封装成工具类（也就是ORM不能很好的完成）。
## 竞品分析
| 评估维度  / 框架  | Hibernate              | MyBatis            | Spring JDBC       |
| ----------- | ---------------------- | ------------------ | ----------------- |
| 🎯 **性能表现** |                        |                    |                   |
| - SQL优化能力   | SQL语句自动生成，优化空间较小       | 使用原生SQL，优化空间大      | 使用原生SQL，优化空间大     |
| - 缓存机制      | 内置二级缓存，设计完善            | 内置二级缓存，设计完善        | 需要额外实现缓存          |
| - 数据源兼容性    | 支持主流第三方数据源             | 支持主流第三方数据源         | 支持主流第三方数据源        |
| 🔄 **可移植性** |                        |                    |                   |
| - 方言支持      | 内置多种数据库方言<br>自动适配数据库差异 | 需手动适配不同数据库语法       | 需手动适配不同数据库语法      |
| - 数据库迁移     | 迁移成本低，无需改动代码           | 迁移成本高，需调整SQL       | 迁移成本高，需调整SQL      |
| ⚡ **开发效率**  |                        |                    |                   |
| - 配置方式      | XML配置 + 注解             | XML配置 + 注解         | Callback模式        |
| - 学习曲线      | 较陡峭                    | 适中                 | 平缓                |
| - 适用场景      | 复杂领域模型<br>对象关系映射要求高    | 复杂SQL查询<br>性能调优要求高 | 简单CRUD操作<br>轻量级应用 |
# 架构
![image.png](https://img.dirtsai.work/astro-blog/2024/12/73d4274bc4dc4b564363d542b05cfaa2.png)
* 基础支持层
	* **反射模块**：对[[Java反射机制详解]]进行了封装，提供API；对反射操作进行了诸如缓存类的元数据之类的优化
	* **类型转换模块**：1. 别名机制；2. JDBC类型和Java类型转换
	* **日志模块**：集成第三方日志框架
	* **资源加载模块**：封装类加载器，确定其使用顺序；提供加载类问价以及其他资源文件的功能
	* **解析器模块**：1. 对[[Xpath]]进行封装，对解析映射文件提供支持；2. 处理动态SQL语句中占位符提供支持。
	* **数据源模块**：数据源实现一机第三方数据源集成接口
	* **事务管理**：mybatis自身提供了相应的事务管理和简单实现；一般与spring集成由spring来管理事务
	* **缓存模块**：提供一级缓存和二级缓存；
		* P.S. MyBatis中自带的这两级缓存与MyBatis以及整个应用是运行在同一个JVM中的，共享同一块堆内存
	* **binding模块**：本质上就是引入 Mapper 接口来替代直接写字符串调用的方式
* 核心处理层
	* **插件** ： 自定义插件对mybatis进行扩展
	* **配置解析**：mybatis-config.xml配置文件、映射配置文件以及Mapper接口中的注解信息，解析后的配置信息会形成相应的对象并保存到Configuration对象中
	* **SQL解析与scripting模块**：1. 动态SQL语句，提供`<where>`等节点 2. scripting模块会根据用户传入的实参，解析映射文件中定义的动态SQL节点，并形成数据库可执行的SQL语句。之后会处理SQL语句中的占位符，绑定用户传入的实参。
	* **SQL执行**：
		* `Executor`：负责维护缓存，事务管理的相关操作；会讲数据库相关操作委托给`StatementHandler`完成
		* `StatementHandler`：通过java.sql.Statement对象执行SQL语句并得到结果集
		* `ParameterHandler`：完成SQL语句实参绑定
		* `ResultSetHandler`：结果集的映射
![image.png](https://img.dirtsai.work/astro-blog/2024/12/11b2d0e7b2c2c8d2393b239e61a15bb4.png)
* 接口层
	* 核心是`SqlSession`接口：定义了MyBatis暴露给应用程序调用的API

# 基础支持层
## 解析器模块
### XML解析
#### XML解析方式
##### DOM（Document Object Model）
* 基于树形结构，将整个XML读入内存建立一个DOM树
	* 元素节点
	* 属性节点
	* 文本节点
* 优点
	* 易于编程，可以较为方便的在节点间导航
##### SAX(Simple API for XML)
1. 解析机制：
	- 事件驱动，基于回调函数
	- 定义了一组标准的回调函数（如 startElement、endElement、characters 等）
	- 开发者通过实现这些回调函数来处理不同类型的节点
2. 推模式特点：
	- 启动解析后，节点处理的时机由解析器控制
	- 必须按顺序处理所有节点
	- 不能暂停或回退解析过程
* 缺点：
	* 维护节点间关系较为复杂
	* 流式处理是单向的，无法重新处理已经处理过的
	* 只读的
	* 无法支持XPath
##### StAX
JDK6引入的[[Java 6 特性|StAX]], 
### XPathParser
`XPathParser`是Mybatis中用于解析XML配置文件的核心类
1. 创建解析器：
	* `XPathParser parser = new XPathParser(inputStream, true, props, null);`
2. 通过`createDocument()`方法创建Document对象用来保存XML的文档结构
3. 使用`eval*() `方法解析节点
```java
// 解析各种节点都是通过 eval*() 方法：
XNode configNode = parser.evalNode("/configuration");  // 解析根节点
XNode envNode = parser.evalNode("environments/environment");  // 解析子节点
List<XNode> propNodes = parser.evalNodes("property");  // 解析多个节点
String value = parser.evalString("@value");  // 解析属性值
```
* `eval*()`方法内部实际上是：
	* 使用 XPath 定位节点
	- 将节点封装为 XNode
	- 处理节点中的变量表达式
#### evalString 和 PropertyParser
```java
public String evalString(String expression) {
    try {
        // 1. 先用 XPath 获取原始字符串值
        String value = (String) xpath.evaluate(expression, document, XPathConstants.STRING);
        // 2. 通过 PropertyParser 处理字符串中的变量
        return PropertyParser.parse(value, variables);
    } catch (Exception e) {
        throw new BuilderException("Error evaluating XPath.", e);
    }
}

public class PropertyParser {
    // 主方法
    public static String parse(String string, Properties variables) {
        // 创建变量处理器
        VariableTokenHandler handler = new VariableTokenHandler(variables);
        // 创建通用解析器，指定变量的开始和结束标记
        GenericTokenParser parser = new GenericTokenParser("${", "}", handler);
        // 解析并返回结果
        return parser.parse(string);
    }
}
```
#### 为什么在xml解析阶段（mybatis刚启动时）就处理${}?
* 因为配置文件如下所示， 在启动阶段就要确定这些诸如数据库连接信息，驱动的值`${jdbc.driver}`
* 还有`${username:root}`这种默认值：
	* 首先`GenericTokenParser.parse()`解析`${}`, 调用 `VariableTokenHandler.handleToken()`解析`:`得到冒号分割的变量名和默认值

```xml
<!-- mybatis-config.xml -->
<configuration>
    <properties>
        <property name="jdbc.driver" value="com.mysql.jdbc.Driver"/>
        <property name="jdbc.url" value="jdbc:mysql://localhost:3306/test"/>
    </properties>
    
    <environments default="development">
        <environment id="development">
            <dataSource type="POOLED">
                <!-- 这里使用 ${} 引用上面定义的属性 -->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

* 而`#{}` 用于 SQL 中的**参数绑定**，在运行时处理

### 反射模块
mybatis对常见的反射做了封装，提供更简洁方便的反射API
# 工作流程
使用示例：[MyBatisDemo](https://github.com/czllll/handwrite/blob/main/src/main/java/work/dirtsai/basic/demoProj/MyBatisDemo.java)
1. 读取配置文件
	*   ```String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);```
	* 读取Mybatis配置文件[mybatis-config.xml](https://github.com/czllll/handwrite/blob/main/src/main/resources/mybatis-config.xml)和映射文件[Mapper.xml](https://github.com/czllll/handwrite/blob/main/src/main/resources/work/dirtsai/basic/demoProj/mapper/UserMapper.xml)
		* **mybatis-config.xml**
			* `<transactionManager type="JDBC"/>`
				* 有两种type：
					* JDBC ：直接使用JDBC的事务管理机制（JDBC Connection对象）；由Mybatis直接管理事务；适合单独使用Mybatis的项目
					* MANAGED：不直接负责，将事务管理交给外部容器（如Spring、JEE容器）来管理；当使用Spring集成mybatis时，不需要配置Mybatis的事务管理器，直接使用[[Spring事务]]，这种情况下，这个配置会被Spring覆盖。
			* `<mappers> <mapper resource="xxxMapper.xml"/> </mappers>`
				* 配置Mapper的路径，可以配置多个Mapper文件
		* `getResourceAsStream` 方法来源于`ibatis.io.Resources`类，这是一个重载方法，最后调用到：
			* `InputStream getResourceAsStream(String resource, ClassLoader[] classLoader)`这个方法， 遍历`classLoader` 并使用其`getResourceAsStream`方法得到输入流
				* 
	* 构建configuration对象，包含所有配置信息如数据源，事务管理器等
* 

1. 构建会话工厂
	* 通过Configuration对象创建SqlSessionFactory对象
	* SqlSessionFactory 是一个单例对象，负责创建SqlSession
2. 创建会话对象
	* 通过SqlSessionFactory创建SqlSession对象
	* SqlSession是一个非线程安全的对象，每次使用完都要及时关闭
3. 执行具体操作
	* 获取Mapper接口的代理对象
	* 当调用Mapper接口的方法时：
		* 将请求转发给SqlSession
		* SqlSession根据配置信息找到对应的SQL
		* 解析SQL中的参数，创建PreparedStatement
		* 执行SQL并处理结果集
		* 将结果集映射为Java对象返回
4. 结果映射和返回
	* 根据ResultMap的配置将结果映射成Java对象
	* 处理一对一，一对多等复杂映射关系
	* 返回映射后的对象给业务层
5. 关闭会话
	* 操作完成后关闭SqlSession
	* 释放数据库连接等资源

