# WinRPC
一个基于windows共享内存的进程间通信库

创作原因：

1.在大部分情况下，使用TCP(select iocp)能够满足大部分的进程间通信的需求，并且更加简单

2.在一些恶劣的PC环境下(网吧环境、广告软件、流氓软件环境等)，TCP或管道会受到代理软件的影响(2345等)，不能够正常工作

3.此时采用共享内存作为进程间通信的基础，能够保持更好的兼容性、容错性

4.需要更加灵活的方式进行消息的分发(例如后端的MQ的订阅方式等)

# WinRPC支持的工作方式

## 1. 1对1进行通信(1服务端 <=> 1客户端)
场景：1服务端与1客户端通信，类似于TCP中的点对点单实例阻塞式通信
## 2. 1对N进行通信(channel模式，1服务端 <=> N客户端)
场景：1服务端与多个客户端通信，类似于TCP的选择模型(select)、完成端口(IOCP)等通信方式
## 3. M对N进行通信(route模式, M服务端 <=> N客户端)
场景1：1服务端与多个客户端进行通信，类似于channel模式
场景2：多个服务端与多个客户端进行通信，服务端只能与客户端进行通信，客户端也只能与服务端进行通信。服务端能以定向发送和广播发送的方式发送数据，客户端也能以定向发送的方式发送数据。
场景3：服务端能够动态加入，动态删除，新启动一个服务器后，只要指定RouteManager的名称是一样的，客户端将自动识别新加入的服务器，然后与该服务器进行通信。