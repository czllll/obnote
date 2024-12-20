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