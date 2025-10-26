
| domain | layer | type  |
| ------ | ----- | ----- |
| #java  | #     | #fact |
* Servlet是一种运行在Web服务器上的小型Java程序，用于动态生成网页内容，处理表单程序等
* 是Jakarta EE规范的一部分
## 生命周期
- **加载**：当客户端请求第一次到达时，Web 容器会加载 Servlet 类并实例化。
- **初始化**：在实例化之后，Web 容器会调用 `init()` 方法进行初始化，这个方法只调用一次。
- **服务请求**：每次客户端请求时，Web 容器会调用 `service()` 方法来处理请求。`service()` 方法根据 HTTP 请求的类型（GET、POST 等）调用相应的 `doGet()`、`doPost()` 等方法。
- **销毁**：当 Web 应用停止时，Web 容器会调用 `destroy()` 方法销毁 Servlet。这个方法也只会调用一次。