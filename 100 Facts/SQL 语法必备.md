
| domain | layer | type  |
| ------ | ----- | ----- |
| #db    | #     | #fact |
## 外键约束
```sql
	CONSTRAINT <约束名>
	FOREIGN KEY (<子表字段>)
	REFERENCES <父表名>(<父表字段>)
	ON DELETE <行为>
	ON UPDATE <行为>;
```
ON行为只可以写在 FOREIGN KEY 约束后面，表示：当父表被删除或更新时，子表要怎么联动处理
`CONSTRAINT <约束名>`是可以省略的
* CASCADE
	* 级联：父表删除/更新时，子表也自动删除/更新对应行
* SET NULL
	* 父表删除/更新时，子表外键字段被置为 NULL
* RESTRICT
	* 阻止删除/更新（默认），若子表存在引用则报错
* NO ACTION
	* 同 RESTRICT（MySQL 中两者等价）
