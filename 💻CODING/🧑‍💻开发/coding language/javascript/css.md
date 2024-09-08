* 盒子模型
	* 从内到外：content->padding->border->margin
	* ![[Pasted image 20240902135159.png]]
	* **margin塌陷**：多个盒子模型连接时，只会取最大的margin
	* margin: 5px 10px 5px 10px; 上右下左的顺序
	* margin: 5px 10px; 5是上下 10是左右
	* margin：5px 上下左右都是5
	* margin: 5px 10px 5px; 上 左右 下
* position
	* static
	* relative
	* absolute
	* fixed
	* sticky
	* 