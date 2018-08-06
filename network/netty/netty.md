#### Netty(Java)
```
Netty是JBoss出品的高效的Java NIO开发框架。

Netty is an 
  - asynchronous 
  - event-driven 
network application framework 
for rapid(快速的) development of 
  - maintainable(可维护性) 
  - high performance
protocol servers & clients.

特性：
  1).asynchronous(异步的) 
    NIO 
  2).event-driven(事件驱动) 
  
   It greatly(非常) simplifies(简单化)
   and streamlines(精简) network programming such as 
   TCP and UDP socket server.

   Netty has succeeded to find a way to achieve 
     ease of development, (开发简单)
     performance, (高性能)
     stability, (稳定性)
     and flexibility (灵活性)
     without a compromise. (又不妥协什么)
优势：
  1).rapid development(快速开发)
    Quick and easy development
  2).maintainable(可维护性)
  3).high performance(高性能)

```

- 1.架构图
```
Transport Services: TCP UDP HTTP
  Socket & Datagram
  Http Tunnel(通道)
  in-vm pipe 虚拟机管道
Protocol Support:
  Http & WebSocket
  SSL(Security Socket Layer 安全套接字层) 
    & TLS(Transport Layer Security 传输层安全性协定)
  Google Protobuf (Google Protocol Buffer)  后缀为.proto 
    一种轻便高效的结构化数据存储格式,结构化数据序列化,很适合做数据存储或 RPC 数据交换格式。

  zlib/gzip Compression(压缩)
  Large File Transfer (大文件转移)
  RTSP (Real Time Streaming Protocol, 实时流协议)
  Legacy Text (遗留文本)
    & Binary Protocols (二进制协议)
Core
  Extensible(可扩展的) Event Model
  Universal(通用的) Commnunication(通信) API
  Zero-Copy-Capable(零拷贝能力) Rich(丰富的) Byte Buffer
```

- 2.高性能的三个主题：传输，协议，线程
```
1).传输: IO模型
2).协议: 通信的数据协议
3).线程: Reactor线程模型
```