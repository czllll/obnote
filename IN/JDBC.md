#database #java

JDBC is a specification from [Sun Microsystems](https://en.wikipedia.org/wiki/Sun_Microsystems) that provides a standard abstraction(API or Protocol) for Java applications to communicate with various databases.
# Components
## JDBC API
* It provides various methods and interfaces for easy communication with the databases. It allows Java programs to execute SQL statements and retrieve response through JDBC drivers.
* 2 packages:
	* java.sql:
	* jakarta.sql
### Interface of JDBC API
- Driver interface
	-  all JDBC driver would implement this
- Connection interface
	- Represents a session with a database
	- Used to create `Statement`, `PreparedStatement` and `CallableStatement` objects.
- Statement interface
	- Used to execute **static** SQL statements, returns query results.
- PreparedStatement interface
	- Sub-interface of `Statement`
	- Used to execute **precompiled** SQL statements.
	- Supports **parameterized queries** to prevent SQL injection.
- CallableStatement interface
- ResultSet interface
	- Represents database query results
	- 由各个数据库具体实现。
	- 不是简单的map接口
		- 有指针概念， 可以一行一行移动
		- 每行数据按列索引组织
		- 提供类型转换功能
		- 支持流式读取
- ResultSetMetaData interface
- DatabaseMetaData interface
- RowSet interface
### Classes of JDBC API
- DriverManager class
- Blob class
- Clob class
- Types class
## JDBC Driver manager
* Manages drivers
	* Maintains all registered database drivers
* Select appropriate driver
	* Choose the right driver based on URL
* Established connection:
	* Creates database connection using the selected driver
```java
Class.forname("com.mysql.cj.jdbc.Driver");

Connection conn = DriverManager.getConnection(
	"jdbc:mysql://localhost:3306/mydb", // database URL 
	"username", 
	"password" 
)
```
## JDBC Test suite
* Test the operation(insertion, deletion...) being performed by JDBC Drivers
## JDBC-ODBC Bridge Drivers
* If there is no JDBC drivers for specific database in some very old systems. We use this bridge drive to connect to database through ODBC.
# JDBC drivers
[[JDBC Drivers]] are client-side adapters that convert requests from Java programs to a protocol that the DBMS can understand.

# Sample
[JDBCDemo.java](https://github.com/czllll/handwrite/blob/main/src/main/java/work/dirtsai/basic/jdbc/JDBCDemo.java)
# Q&A
## What's the different between *Pre-Compiled SQL* and *Static* one?

```java
// Unsafe: Static SQL
String userInput = "' OR '1'='1"; // Malicious input
Statement stmt = connection.createStatement();
String sql = "SELECT * FROM users WHERE name = '" + userInput + "'";
// Results in: SELECT * FROM users WHERE name = '' OR '1'='1'
// This will return all users!

// Safe: Precompiled SQL
PreparedStatement pstmt = connection.prepareStatement("SELECT * FROM users WHERE name = ?");
pstmt.setString(1, userInput);
// Safely escapes the input, prevents SQL injection
```
- **Security**: PreparedStatement is safer
- **Performance**: PreparedStatement is faster for repeated executions
- **Maintainability**: PreparedStatement code is cleaner and easier to read



