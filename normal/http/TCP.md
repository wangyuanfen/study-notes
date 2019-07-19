## TCP三次握手
建立连接前，客户端和服务端需要通过握手来确认对方:
* 客户端发送 syn(同步序列编号) 请求，进入 syn_send 状态，等待确认
* 服务端接收并确认 syn 包后发送 syn+ack 包，进入 syn_recv 状态
* 客户端接收 syn+ack 包后，发送 ack 包，双方进入 established 状态
## TCP四次挥手
* 客户端 -- FIN --> 服务端， FIN—WAIT
* 服务端 -- ACK --> 客户端， CLOSE-WAIT
* 服务端 -- ACK,FIN --> 客户端， LAST-ACK
* 客户端 -- ACK --> 服务端，CLOSED