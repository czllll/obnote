#js
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
		* `Array.prototype.map()` 方法的工作原理：
		    - ```array.map((currentValue, index, array) => { ... })```
		    - `map()` 方法在遍历数组时，会为回调函数提供三个参数：
		        1. 当前元素的值
		        2. 当前元素的索引
		        3. 正在遍历的数组本身
		- 
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
### promise
* 用来处理异步操作的工具，通俗地说，Promise就像一个承诺，它表示某个操作将在未来某个时刻完成，并且会产生一个结果或错误。
#### 创建promise
```js
let promise = new Promise(function(resolve, reject){
	// 异步操作，比如网络请求或定时器 
	const success = true; // 假设操作成功 
	if (success) { 
		resolve('操作成功'); 
	} else { 
		reject('操作失败'); 
	}
});
```
* promise的两种状态![[Pasted image 20240830171239.png]]
#### 消费者：then catch
* 使用then处理成功的结果
	* 当promise被成功resolve时，then方法会被调用第一个函数，并且会接收到resolve传递的值
	* 第二个参数当promise被reject时被调用
```js
promise.then(
  function(result){...}
  function(error){}
)
```
`.catch(f)` 调用是 `.then(null, f)`
#### finally 清理
* finally 总是会在promise状态变为settled（fulfilled或rejected）时首先执行，即在promise结束无论成功还是失败时被调用，用于执行清理操作，then在finally执行完之后才会执行
#### promise API
* Promise类中有6种静态方法
##### promise.all
* 希望并行执行多个promise，并等待所有promise都准备就绪
`let promise = Promise.all(iterable)`
##### promise.allSettled
* 如果任意的promise reject,则整个Promise.all整个将会reject
##### promise.race
* 与Promise.all类似，但只等待第一个settled的promise并获取结果或error
##### promise.any
* 与promise.race类似，但是等待第一个fulfilled的promise，并将这个fulfilled的promise返回
##### promise.resolve/reject
* 基本不使用，因为有async/await

#### promisification
* 核心是将一个基于回调的函数包装成返回 `Promise` 的函数
* 可使用Node.js中的`util.promisify`来进行自动化
```js
const readFile = util.promisify(fs.readFile); 
readFile('example.txt', 'utf8') 
	.then(content => { console.log(content); }) 
	.catch(error => { console.error(error); });
```
#### 微任务
* 一种异步任务，执行优先级高于普通的宏任务，通常是在当前**事件循环**的一轮执行完后立即执行的，确保在下一个事件循环开始之前完成。
##### 来源
* promise
	* Promise的then catch finally回调会被添加到微任务队列中
* MutationObserver
	* 监控DOM变化的MutationObserver回调也是微任务
* queueMicrotask
	* 显式将一个函数添加到微任务队列中
##### 宏任务是什么？
* 另一类异步任务，包含常见setTimeout、setInterval，IO操作，DOM事件等
### async/await
* 为了使异步代码更容易书写和理解，使其更像同步代码
#### async关键字
* 声明一个函数为异步函数，async函数会返回一个promise对象，即使你在函数中直接返回一个值，这个值也会包装在一个promise中
```js
async function example(){
	return "hello"
}

let example = new Promise(function(resolve, reject){
	resolve("hello)
});
//两者相同
```
#### await关键字
* 只能在async中使用，会暂停函数的执行，直到promise完成，并返回其结果
```js
async function fetchData() {
  let response = await fetch('https://api.example.com/data');
  let data = await response.json();
  console.log(data);
}
```

## Generator
* 生成器是一种特殊类型的函数，允许函数在执行过程中暂停和恢复，通过`yield`关键字生成多个值，生成器函数的调用不会立即执行，而是返回一个生成器对象，该对象可以通过调用'next()'方法逐步执行
```js
function* myGenerator() { yield '第一项'; yield '第二项'; }
const gen = myGenerator(); 
console.log(gen.next()); // { value: '第一项', done: false } 
console.log(gen.next()); // { value: '第二项', done: false } 
console.log(gen.next()); // { value: undefined, done: true }
```
### 异步迭代
* 用于处理需要逐步获取的数据流
#### 异步生成器
```js
async function* asyncGenerator() {
  yield await new Promise(resolve => setTimeout(() => resolve('First'), 1000));
  yield await new Promise(resolve => setTimeout(() => resolve('Second'), 1000));
  yield await new Promise(resolve => setTimeout(() => resolve('Third'), 1000));
}

```
#### for wait... of
```js
(async () => {
  for await (let value of asyncGenerator()) {
    console.log(value);
  }
})();

```

## 导入
### 动态导入
* import(module)家在模块并返回一个promise
## Proxy和Reflect
#todo 
## Document
### 文档对象模型DOM
* 将所有页面内容表示为可以修改的对象
#### DOM树
* `<html> = document.documentElement`
* `<body>` = `document.body`
* `<head>` = `document.head`
##### 搜索
* `document.getElementById(id)`
* `elem.querySelectorAll(css)`:返回与指定css选择器匹配的所有元素的NodeList
* `elem.matches(css)`:检查elem是否与给定的css选择器相匹配，返回true或false
* `elem.closest(css)` 方法会查找与 CSS 选择器匹配的最近的祖先。`elem` 自己也会被搜索。
##### 节点属性
![[Pasted image 20240831142343.png]]
* nodeName和tagName
	* 给定DOM节点可以从nodeName或者tagName属性读取标签名
	* tagname仅适用于Element节点
	* nodeName适用于任何Node
* innerHTML
	* 将元素的HTML获取为字符串形式
* outerHTML
	* 包含元素本身的完整HTML
* nodeValue/data：文本节点的内容
	* 两者只有规范上细微差别
* textContent：纯文本
	* 对元素内文本的访问权限
* hidden属性
	* 指定元素是否可见 true/false


#### 特性和属性
* 浏览器加载页面会读取HTML并从中生成DOM对象，大多数特性属性会一一对应
	* 标签《特性》：`<body id="page">`-> DOM 对象《属性》: `body.id="page"`
* HTML特性特征
	* 大小写不敏感
	* 值是字符串类型
#### DOM修改
* 创建新元素节点：`document.createElement(tag)`
* 创建文本节点：`document.createTextNode(text)`
* 元素插入
	* `node.append(...nodes or strings)` —— 在 `node` **末尾** 插入节点或字符串，
	- `node.prepend(...nodes or strings)` —— 在 `node` **开头** 插入节点或字符串，
	- `node.before(...nodes or strings)` —— 在 `node` **前面** 插入节点或字符串，
	- `node.after(...nodes or strings)` —— 在 `node` **后面** 插入节点或字符串，
	- `node.replaceWith(...nodes or strings)` —— 将 `node` 替换为给定的节点或字符串。
#### 样式和类
* 两种方式添加样式
	* 在 CSS 中创建一个类，并添加它：`<div class="...">`
	* 将属性直接写入 `style`：`<div style="...">`
#### 元素大小
![[Pasted image 20240831144950.png]]
### 浏览器对象模型BOM
* 由浏览器提供的用于处理文档之外的所有内容的其他对象

## 事件
* 是某事发生的信号，所有的DOM节点都生成这样的信号（不限于DOM）
### 事件类别
* 鼠标事件
	* - `click` —— 当鼠标点击一个元素时（触摸屏设备会在点击时生成）。
	- `contextmenu` —— 当鼠标右键点击一个元素时。
	- `mouseover` / `mouseout` —— 当鼠标指针移入/离开一个元素时。
	- `mousedown` / `mouseup` —— 当在元素上按下/释放鼠标按钮时。
	- `mousemove` —— 当鼠标移动时。
- 键盘事件：
	- `keydown` 和 `keyup` —— 当按下和松开一个按键时。
* 表单（form）元素事件：
	- `submit` —— 当访问者提交了一个 `<form>` 时。
	- `focus` —— 当访问者聚焦于一个元素时，例如聚焦于一个 `<input>`。
* Document 事件：
	- `DOMContentLoaded` —— 当 HTML 的加载和处理均完成，DOM 被完全构建完成时。
* CSS 事件：
	- `transitionend` —— 当一个 CSS 动画完成时。
### 事件响应
#### HTML特性
* 如onclick
#### DOM属性
* 如onclick
#### addEventListener
* `element.addEventListener(event, handler,[, options]);`
	* event 事件名 如click
	* handler 处理程序
	* option
		* once：true则代表被处罚后自动删除监听器
		* capture：事件处理阶段
		* passive：若为true，处理程序不会调用preventDefault()
* `element.removeEventListener(event, handler[, options]);`
#### 冒泡
* 是一种时间传播机制，指事件从最具体的元素（目标元素）开始触发，然后逐级向上传播到较不具体的节点（父级元素）的过程
* 阻止冒泡
	* event.stopPropagation()
##### 事件传播的三个阶段
* 捕获阶段：事件从window向下走近元素
* 目标阶段：事件到达目标元素
* 冒泡阶段：事件从元素开始冒泡
完整链路：点击某个元素，事件手贱通过祖先链向下到达元素，到达目标，最后上升，在途中调用处理程序

#### 事件委托
* 若有许多以类似方式处理的元素，不必为每个元素分配一个处理程序———— 而时间耽搁处理程序放在他们的共同祖先上·。

### 浏览器行为
* 阻止浏览器行为
```js
<a href="/" onclick="return false">Click here</a>
or
<a href="/" onclick="event.preventDefault()">here</a>
```

## 接口
### 可读流
* Node.js中提供了一组操作和事件来处理数据流