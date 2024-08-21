# 简介
## 运行环境
* 可在浏览器、服务端执行
* 在搭载了Javascript引擎（专门处理Javascript脚本的虚拟机）的设备中执行
	* V8 -- Chrome 、 Edge
	* SpiderMonkey -- Firefox
## Node.js是什么
* Node.js是一个使用了V8引擎的js运行环境，允许在浏览器之外执行js代码
* 类似的js的运行环境有浏览器、[Deno](https://zh.wikipedia.org/wiki/Deno)等
## 编码习惯
* 每条语句加分号
* js引擎并为假设[]前会有分号，因此若上一条语句未加分号会报错
* 现代模式：确保该条语句出现在脚本最顶部或函数体的开头；若使用class 和 module时，默认启用现代模式
```javascript
"use strict"
//代码以现代模式工作
```
## 基础
### 变量
#### 变量命名
* 采用驼峰
* `'$'` 和下划线 `'_'`也可用作命名
* 区分大小写
* 保留字如 let、class 不允许命名
#### 常量命名
* 使用`const`
* 通常使用常量用作**别名**，一半是在代码执行前就已知的值
	* 使用大写字母和下划线来命名这些常量：`const` `COLOR_RED` `=` `"#F00"``;
### 数据类型
* `typeof`运算符返回参数的类型
```js
typeof true // "boolean"
```
#### 原始类型
* Number类型
	* 特殊数值
		* `Infinity`
		* `-Infininty`
		* `NaN`:代表计算错误
			* 特殊的：`NaN ** 0 == 1`
* BigInt类型
	*  Number是基于IEEE 754 双精度浮点数标准的，“number” 类型无法安全地表示大于 $(2^{53}-1)$，或小于 $-(2^{53}-1)$ 的整数。
	* BigInt用于表示任意长度整数，将`n`添加到末尾来创建BigInt值
* String类型
	* 单引号和双引号：简单引用，几乎无差别
	* 反引号：代表*功能扩展*引号，允许将变量和表达式包装在`${...}`中
```javascript
alert( `the result is ${1 + 2}` ); 
```
* Boolean类型
	* true/false
* null值
	* 独立类型，只包含null值
	* 并非空引用或null指针，而是代表“空”，“无”
* undefined值
	* 变量被声明但未被赋值
* Symbol类型
	* 用于创建对象的唯一标识符
#### 引用类型
* Object类型
	* 用于存储集合和更复杂的实体