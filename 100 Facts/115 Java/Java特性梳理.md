---
tags:
- java
- jdk
---
# Java 6
## StAX(Streaming API for XML)
* JAXP是JDK提供的一套用于解析XML的API，支持DOM和SAX解析方式
* JDK6 开始 支持StAX
1. 解析机制：
	- 迭代器模式
	- 开发者主动调用方法获取下一个节点
	- 可以根据需要选择处理或跳过节点
2. 拉模式特点：
	- 启动解析后，节点处理的时机由开发者控制
	- 可以随时暂停解析
	- 能灵活控制处理流程

# Java7
# try-with-resources
用于自动关闭实现了`AutoCloseable`接口的资源。
**传统写法**
```java
public static void queryDataT(Connection connection) {  
    Statement statement = null;  
    ResultSet rs = null;  
  
    String sql = "SELECT * FROM users";  
    try {  
        statement = connection.createStatement();  
        rs = statement.executeQuery(sql);  
        while (rs.next()) {  
            System.out.printf("ID: %d, 姓名: %s, 年龄: %d, 邮箱: %s%n",  
                    rs.getInt("id"),  
                    rs.getString("name"),  
                    rs.getInt("age"),  
                    rs.getString("email")  
            );  
        }  
    } catch (SQLException e) {  
        throw new RuntimeException(e);  
    } finally {  
        // Close in reverse order of creation  
        if (rs != null) {  
            try {  
                rs.close();  
            } catch (SQLException e) {  
                e.printStackTrace();  
            }  
        }  
  
        if (statement != null) {  
            try {  
                statement.close();  
            } catch (SQLException e) {  
                e.printStackTrace();  
            }  
        }  
    }  
}
```
**try-with-resources 写法**
```java
private static void queryData(Connection conn) throws SQLException {  
    String sql = "SELECT * FROM users";  
  
    try (Statement stmt = conn.createStatement();  
         ResultSet rs = stmt.executeQuery(sql)) {  
  
        System.out.println("\n查询结果:");  
        while (rs.next()) {  
            System.out.printf("ID: %d, 姓名: %s, 年龄: %d, 邮箱: %s%n",  
                    rs.getInt("id"),  
                    rs.getString("name"),  
                    rs.getInt("age"),  
                    rs.getString("email")  
            );  
        }  
    }  
}
```
通过对比我们可以看出**区别**：
* 传统写法要手动按顺序关闭创建的资源，且不够简洁
* 每个close()都可能会抛出异常，都需要异常处理

# Java 9
## Class.newInstance()方法废弃
* 官方注释
	* This method propagates any exception thrown by the nullary constructor, including a checked exception. Use of this method effectively bypasses the compile-time exception checking that would otherwise be performed by the compiler. The `Constructor. newInstance` method avoids this problem by wrapping any exception thrown by the constructor in a (checked) InvocationTargetException.
	* 意思说如果构造函数抛出了一个`InstantiationException`或`IllegalAccessException`以外的受检查异常,这个异常会从`Class.newInstance()`方法中抛出,但是编译器不会强制调用者处理这个异常,因为它没有在`Class.newInstance()`的声明中指定，比较有风险；在其声明中指定了`InvocationTargetException`异常。这个异常包装了构造函数抛出的任何异常。这迫使调用者必须处理`InvocationTargetException`,从而间接地处理构造函数可能抛出的任何异常。
	* The call `clazz.newInstance()` can be replaced by `clazz.getDeclaredConstructor().newInstance()`
	* The latter sequence of calls is inferred to be able to throw the additional exception types `InvocationTargetException` and `NoSuchMethodException`. Both of these exception types are subclasses of `ReflectiveOperationException`.



# Jdk 14
## Dual-pivot quicksort improvements
* [[JDK-8226297] Dual-pivot quicksort improvements - Java Bug System](https://bugs.openjdk.org/browse/JDK-8226297)
* 对于双轴快排进行了改进

# Java 16
## record
* jdk14 预览版，jdk16正式引入
* 自动生成一个final 类
```java
public record Point(int x, int y) {}
// 等价于
public final class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() { return x; }
    public int y() { return y; }

    @Override
    public boolean equals(Object o) { ... }

    @Override
    public int hashCode() { ... }

    @Override
    public String toString() {
        return "Point[x=" + x + ", y=" + y + "]";
    }
}

```