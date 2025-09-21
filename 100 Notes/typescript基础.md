#js
# interface
## 概念
* 用于定义对象的结构和类型
* 接口只存在于ts的类型系统中，在编译为js后不会产生任何代码
## 用途
* 类实现：类可以实现接口，确保类包含接口定义的所有成员
## 特性
* 可选属性：使用?标记
* 只读属性：使用readonly标记
* 函数类型：可以定义函数的签名
```ts
interface Demo {
	model?: string;
	readonly x: number;
	
}
```

# type
* 关键字，用于创建类型的别名或定义自定义类型

# 解构赋值

# 泛型
* 在ts中，泛型允许我们为函数，类，接口定义类型参数，以便能够根据不同的类型进行灵活的代码重用。
* 例如`useForm<z.infer<typeof formSchema>>({...})`,就是userForm钩子的泛型参数，
	* `z.infer<typeof formSchema>` 是Zod提供的类型推断工具，根据Zod模式来自动推断出表单数据的类型
