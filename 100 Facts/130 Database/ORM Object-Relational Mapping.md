---
tags:
- db
---
ORM 即对象关系映射
ORM框架的主要功能就是根据映射配置文件，完成数据在对象模型与关系模型之间的映射，同时也屏蔽了[[JDBC基础]]的代码，只暴露简单的API供开发人员使用。
![image.png](https://img.eryi.me/astro-blog/2025/10/b4e503e893bb988e1f4758e5aa61301c.png)
常见的 ORM 框架如 Java 生态有 Hibernate、MyBatis 、JPA 等；Python 有 Django ORM 等

## N+1问题
是 ORM 中一个经典的性能瓶颈问题。代码像操作对象一样访问关联数据，而忽略了底层数据库的查询逻辑。
为了获取一组目标数据及关联数据，系统执行了 1 次初始查询和 N 次后续查询
### 解决
核心思路就是将分散的 N 次查询合并为一次大查询
如：
* `join` ：让 ORM 第一次查询就通过`join`把关联数据都带回来
* 批量查询：将 N 次查询减少到 N/BatchSize 次
* 使用 DTO 投影Data Transfer Object，数据传输对象：根据业务具体需求，定义一个只包含必要字段的”瘦对象“，直接通过 sql 填充
	* PS：业界实践一般是写操作使用 BO 保证领域逻辑的一致性和完整性；读操作使用 DTO 投影获得最佳性能