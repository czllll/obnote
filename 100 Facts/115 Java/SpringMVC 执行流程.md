---
tags:
- java
- spring
---
1. 用户发出请求到前端控制器Dispatcher[[Servlet]]
2. DispatcherServlet收到请求调用HandlerMapping（处理器映射器）
3. HandlerMapping找到具体的处理器，返回处理器执行链（包括处理器和拦截器）给DispatcherServlet
4. DispatcherServlet调用HandlerAdapter（处理器适配器）
5. HandlerAdapter经过适配调用具体的处理器（Handler即Controller的某个方法）
6. 在方法上添加了@ResponseBody，因此通过HttpMessageConverter来返回结果转换为JSON并响应

![image.png](https://img.eryi.me/astro-blog/2025/10/4ab1112e30bcd248bf8e3f2dba7a4c6c.png)
### DispatcherServlet
* 继承自 HttpServlet
* HttpServlet 继承自Servlet
* 普通的 Servlet 负责接受请求，处理并返回相应，通常会直接处理请求并生成相应内容如 HTML 页面，开发者需要在 doGet()或 doPost()等方法中编写请求处理的具体逻辑
* DispatcherServlet不直接处理业务逻辑，而是负责将实际请求发给实际的业务处理组件（如 Controller），DispatcherServlet会使用 Spring 配置的 **HandlerMapping** 来找到请求对应的处理方法，然后通过 **HandlerAdapter** 来执行该方法，最后返回视图或结果。

### HandlerMapping
* 是 Spring MVC 中的一个接口，用于将 HTTP 请求的 URL 映射到一个处理该请求的 Controller 方法
* 返回对应的 Handler（通常是 Controller 方法）


### HandlerAdapter
* 职责
	* 调用 Handler：执行找到的 Handler 方法
	* 返回结果：执行完 Controller 方法后，会将结果传递会 DispatcherServlet，通常返回一个 ModelAndView 对象或一个直接的视图和数据