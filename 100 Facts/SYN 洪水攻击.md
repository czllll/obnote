
| domain | layer | type   |
| ------ | ----- | ------ |
| #net   | #     | #fact  |
* 假设攻击者短时间伪造不同 IP 地址的 `SYN` 报文，服务端每接收到一个 `SYN` 报文，就进入`SYN_RCVD` 状态，但服务端发送出去的 `ACK + SYN` 报文，无法得到未知 IP 主机的 `ACK` 应答，久而久之就会**占满服务端的半连接队列**，使得服务端不能为正常用户服务。
## 避免
- 增大 TCP 半连接队列：通过调整 `tcp_max_syn_backlog` 和 `somaxconn` 参数，扩大半连接队列的容量。
- 开启 tcp_syncookies：启用后，服务器通过加密算法生成 SYN Cookie 作为初始序列号，无需维护**半连接状态**，直接跳过队列限制。
- 减少 SYN+ACK 重传次数：通过 `tcp_synack_retries` 减少 SYN-ACK 的重传次数，加速释放半连接资源