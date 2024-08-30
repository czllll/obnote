#学习 #js
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
	* 使用大写字母和下划线来命名这些常量：`const COLOR_RED = "#F00"`
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
	* num.toString(base)
		* base表示进制数
		* 若num不是变量而是数字，则需要两个点来调用该方法，因为js引擎默认数字后第一个小数点的后为小数部分：``123456.``.``toString``(``36``)``
* BigInt类型
	*  Number是基于IEEE 754 双精度浮点数标准的，“number” 类型无法安全地表示大于 $(2^{53}-1)$，或小于 $-(2^{53}-1)$ 的整数。
	* BigInt用于表示任意长度整数，将`n`添加到末尾来创建BigInt值
* String类型
	* 单引号和双引号：简单引用，几乎无差别
	* 反引号：代表*功能扩展*引号，允许将变量和表达式包装在`${...}`中
		* `alert( `the result is ${1 + 2}` ); `
	* 可使用`for .. of`遍历字符
		* `for (let char of "Hello")`
	* 字符串不可变
      
* Boolean类型
	* true/false
* null值
	* 独立类型，只包含null值
	* 并非空引用或null指针，而是代表“空”，“无”
* undefined值
	* 变量被声明但未被赋值
* Symbol类型
	* 用于创建对象的唯一标识符
	* 
#### 引用类型
##### Object类型
* 用于存储集合和更复杂的实体
###### 数组
* 数组是一种种特殊的对象
* 方法
	* **字符串操作**
		* 栈和队列
			* push/pop:末端进出
			* pus/shift：末端进，首端出
			* unshift：首端进
		* `splice(start, deleteCount, elem1,..., elemN`
			* 从索引start开始修改：删除deleteCount个元素，并在当前位置插入elem1，...,elemN元素
		* `arr1.concat(arr2,arr3)`合并生成新数组
	* **遍历**
		* 使用`arr.forEach`方法允许为数组的每个元素都运行一个函数
			* `arr.forEach(function(item, index, array){...});`
			* `arr.forEach((item, index, array) => {...});`
	* **查找**
		* 要取最后一个元素可以使用`.at(-1)`
		* arr.find() -- 查找数组中满足提供的测试函数的第一个元素
		* arr.filter() -- 查找所有匹配元素组成的数组
	* **map**
		* 对数组每个元素都调用函数，返回结果数组
	* **排序**
		* arr.sort()原位排序；默认按照字符串，可提供参数修改排序方法
* 循环
	* 可使用for或for .. of
	* 不建议是使用for .. in

###### iterable object
* `for .. of`以来对象的可迭代性，普通的js对象并没有默认的迭代器，因此不能直接用`for of`
* 通过实现`Symbol.iterator`方法，对象可转变为可迭代的对象
* 类数组是什么？
	*  **Array-like** 是有索引和 `length` 属性的对象，所以它们看起来很像数组。
	* Array.from()方法接受u一个可迭代或类数组的值，并从中获取一个真正的数组
###### map
* 普通的Object会将key转换为字符串，而map不会
* 迭代方法
	* map.keys(),遍历并发会一个包含所有key的可迭代对象
	* map.value()
	* map.entries(),返回所有实体\[k,v]的可迭代对象
* 创建
	* Object->Map:_`new Map(Object.entries(obj))
	* Map->Object:`Object.fromEntries(map.entries())`
###### Set
* value的集合，每个值只出现一次
* 迭代方法
	* for ... of
	* for each
### 运算符
#### 一元运算符+
* 对数字没有任何作用
* 不是数字则会将其转变为数字,效果和Number()相同
```js
let y = -2;
alert( +y ); // -2

alert( +true ); // 1
alert( +"" ); // 0
```
#### 空值合并运算符 `??`
* 如果左侧操作数是 `null` 或 `undefined`，则返回右侧操作数。
* 出于安全原因，JavaScript 禁止将 `??` 运算符与 `&&` 和 `||` 运算符一起使用，除非使用括号明确指定了优先级。
### 值的比较
#### 严格比较
* 由于两侧会先转化为数字再比较，普通的` == ` 不能区分0 和false；不能区分`''`和`false`
* ` ===`严格相等运算符不会进行任何类型转换
* `undefined`和`null`再 == 不会进行任何类型转换

### 函数
* 在JavaScript中，函数是一种特殊的**值**
* **函数声明**在被定义之前就可以被调用
>* **P.S.** 提升(Hoisting)是指在 JavaScript 中，变量和函数声明会在代码执行前被提升到其所在作用域的顶部。这意味着，无论函数声明出现在代码的什么位置，JavaScript 引擎都会在实际执行代码之前将这些声明“提升”到作用域的顶部。*
#### 变量
* 函数可以访问外部变量，函数内部有同名变量则会遮蔽外部变量
#### 返回
* Javascript默认在return之后加上分号
#### 函数表达式
```js
let sayHi = function(){
	alert("hello");
};
```
#### 箭头函数
```js
let func = (arg1, arg2, ..., argN) => expression;
//两者相同
let func = function(arg1, arg2, ..., argN) {
	return expression;
};

```
## Object：对象
* 创建对象
```js
let user = new Object(); // “构造函数” 的语法
let user = {}; // “字面量” 的语法
```
* 多词属性名需要加上引号
```js
let user = {
name: "John",
"likes birds": true， // 多词属性名必须加引号
};
```
* 列表中的最后一个属性以逗号结尾，被称为*尾随逗号*，被视作一种良好的编码习惯
* 点符号不能正确解释多词属性名，可以使用方括号，效果同点符号
* 计算属性：
```js
let bag = {
  [fruit]: 5, // 属性名是从 fruit 变量中得到的
};
```
### 垃圾回收
* 类似于java的垃圾回收机制，都是基于 **可达性原则**，在js中，全局对象和当前执行栈的变量被称作根，如果一个值可以从根通过引用或者引用链进行访问，则认为该值是可达的。
* 非可达的元素将被垃圾回收算法清除
#### 算法
* mark-and-sweep
### this
* 箭头函数没有自己的this
> *这是箭头函数的一个特性，当我们并不想要一个独立的 `this`，反而想从外部上下文中获取时，它很有用*
### 构造函数
* 构造函数在技术上是普通函数，有两个约定：
	* 命名以大写字母开头
	* 只能由`new`操作符执行
### 对象转化原始值
>`hint` 是 JavaScript 引擎在执行转换时的“提示”，它告诉引擎需要什么类型的原始值。
* 三种类型hint
	* string
		* 当js需要字符串作为结果时，如`alert(obj)`
	* number
		* 如`obj+1`
	* default2
		* 通常出现在比较中（对象与字符串或数字进行比较时）
### json
* 方法
	* `JSON.stringify` 将对象转换为 JSON。
	* `JSON.parse` 将 JSON 转换回对象。


## 函数进阶
### 全局对象
* 在浏览器中是 window，在Node.js中是 global。
* ES11中 globalThis 作为标准名称被加入到js
### 函数对象
* 函数在js中是一种值，类型是对象
* 属性
	* name
	* length，返回入参个数，rest参数不参与计数
### 命名函数表达式 NFE
* 是什么？
	* 带有名字的函数表达式
```javascript
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};
```
* func即为名字，供函数内部调用，不实用外部变量sayHi是因为sayHi在外部的词法作用域中，可能会被改变
### 装饰器模式
* 装饰器可以在不修改源头代码的情况下，动态地对函数进行添加修改增强
#### this绑定规则
* **默认绑定**：函数子啊全局作用域被调用时，this默认指向全局对象
* **隐式绑定**：使用点符号调用对象方法时，this绑定到该对象
* **显式绑定**：通过call(),apply(),bind()方法，可以显式设置this的值
* **箭头函数**：不绑定自己的this，从外部作用域继承this
### call、apply、bind三个方法
#### call
* `func.call(thisArg, arg1, arg2, ...)`
* 立即调用一个函数，并显式指定this的值
#### apply
* `func.apply(thisArg, [argsArray])`
* 立即调用一个函数，使用数组或类数组传递参数
#### bind
* `let boundFunc = func.bind(thisArg, arg1, arg2, ...)`
* 创建一个新的函数，并永久绑定`this`,不会立即调用函数，而是返回一个新函数

## 原型继承
### 原型链
* 当读取/删除对象属性时，js会现在对象本身查找；若未找到则沿着原型链向上查找，直到找到或到顶
### 方法
#### 设置\[\[\Prototype]]
* 对对象
```js
let animal = {
	eat: true
};
let rabbit = {
	jumps: true
}

rabbit.__proto__ = animal; //deprecated
Object.getPrototypeOf(rabbit)
Object.setPrototypeOf(rabbit, animal)
```
* 当使用new F()这样的构造函数创建新对象时,如下，new Rabbit的时候即设置了\[\[\Prototype]]为 animal
```js
Rabbit.prototype = animal;
let rabbit = new Rabbit("White Rabbit");
```
## 类
### Mixin
* 是什么？
	* 是一种设计模式，由于js只能继承一个对象，所以在需要继承多个对象时，Mixin允许你将多个类的功能混合到一个类中
* 实现方法
	* 简单对象扩展
		* ```Object.assign(SomeClass.prototype, someObject)```
	* ...
## Promise、async、await
* 