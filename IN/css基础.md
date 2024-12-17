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

# 伪类
* **CSS 伪类** 是一种用于选择元素特定状态的类，可以应用额外的样式而无需修改元素的 HTML 结构。伪类通常用于选择元素的不同交互状态、特定的子元素或元素的一些特定情况。
* 伪类以冒号 (`:`) 开头
```css
1. 交互状态伪类
# 鼠标悬停触发
button:hover {
  background-color: blue;
}
# 元素获得焦点时触发
input:focus {
  outline: 2px solid blue;
}
# 用户正在点击时触发
button:active {
  background-color: green;
}
# 仅通过键盘导航情况下元素获得焦点时触发
button:focus-visible {
  outline: 2px solid orange;
}
2. 解构伪类：用于选择给予文档结构中的特定元素或子元素
p:first-child {
  font-weight: bold;
}
3. 状态伪类：基于页面状态的元素
：check 选择所有被选中的输入框或复选框
:disabled 选择所有被禁用的表单元素
：require 选择带有 `required` 属性的表单元素
```