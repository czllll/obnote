---
tags: []
---
* STOMP 协议，Simple Text Oriented Messaging Protocol
* 是ws的高层协议
## 结构
* **COMMAND**（命令）：指明消息的类型，如 SEND、SUBSCRIBE、UNSUBSCRIBE、CONNECT 等。
- **HEADERS**（头信息）：包括消息的一些元数据，如目的地址、消息体长度、订阅 ID 等。
- **BODY**（消息体）：实际的消息内容，通常是一个字符串，可能是 JSON 或其他格式。