# 组件
* 是什么？
	* react应用程序是由组件构成的
	* React 组件是返回标签的 JavaScript 函数
```js
function MyButton() {  
	return (  
		<button>I'm a button</button>  
	);  
}
```
**注意**：React组件必须以大写字母开头，而 HTML 标签则必须是小写字母

## 组件返回
* React 组件必须返回单个 JSX 元素，不能像两个按钮那样返回多个相邻的 JSX 元素。要解决此问题，可以使用 Fragment（`<>` 与 `</>`）包裹多个相邻的 JSX 元素
```js
export default function Square() {  
  return (  
    <>  
      <button className="square">X</button>  
      <button className="square">X</button>  
    </>  
  );  
}
```
# hook
* 以use开头的函数被称为hook，允许你在函数组件中访问react特性，如状态和生命周期方法
	- **useEffect**：用于处理副作用，如数据获取和订阅。
	- **useContext**：用于在组件树中共享数据而无需通过 props 逐层传递。
- 注意：
	- 只能在你的组件（或其他 Hook）的 **顶层** 调用 Hook
## useState
```js
const [state, setState] = useState(initialState);
```
* 用于在函数组件中添加状态
* 返回一个数组，其中包含两个元素：
	1. **当前状态**：表示状态的当前值。
	2. **更新状态的函数**：用于更新状态并触发组件重新渲染。


* index.js 是 App.js文件中创建的组件与Web浏览器之间的桥梁，将导入的文件组合在一起，将成果注入public中的index.html文件中、

# react 渲染和提交
* 请求和提供UI的过程
	* 触发
	* 渲染
	* 提交
## 1.触发- 一次渲染
### 原因：
#### 组件 **初次渲染**
* 通过调用 [`createRoot`](https://zh-hans.react.dev/reference/react-dom/client/createRoot) 方法并传入目标 DOM 节点，然后用你的组件调用 `render` 函数完成的：
```js
//index.js
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);

//Image.js
export default function Image() {
  return (
    <img
      src="https://i.imgur.com/ZF6s192.jpg"
      alt="'Floralis Genérica' by Eduardo Catalano: a gigantic metallic flower sculpture with reflective petals"
    />
  );
}
			
```
#### 组件（或其祖先之一）的状态发生改变
* 在组件初次渲染之后，就可以使用set函数更新其状态，更新组件的状态会自动将一次渲染送入队列
## 2.react渲染组件
* 初次渲染：react调用根组件，为标签创建DOM节点
* 后续渲染：react 调用内部状态更新出发了渲染的函数组件
## 3.提交到DOM上
* 初次渲染： React 会使用 [`appendChild()`](https://developer.mozilla.org/docs/Web/API/Node/appendChild) DOM API 将其创建的所有 DOM 节点放在屏幕上。
* 后续渲染：React 将应用最少的必要操作（在渲染时计算！），以使得 DOM 与最新的渲染输出相互匹配。
## 4.浏览器渲染/绘制
* 渲染提交后，浏览器会重新绘制屏幕
# 一些概念
## 保留属性
* **强烈建议你在构建动态列表时分配适当的 key**
## 解构赋值
* 在函数参数中，使用`{ category }`表示从传入的对象中提取名为`category`的属性。这种方式使得代码更加简洁和易读。
## 判断state
* state使组件可以对一些信息保持追踪，并根据交互来改变
* 会随着时间的推移而变化，并且无法从任何东西中计算出来。
