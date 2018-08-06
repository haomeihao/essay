#### Interview
```
数据结构&算法
tcp/ip
sql优化
多线程
http连接(http报文)
jdbc查sql
5层网络模型

```

```
1.TCP/IP 5层网络模型
应用层 HTTP FTP Telnet SMTP DNS
  FTP Telnet SMTP -> TCP支持
  DNS -> UDP支持
  HTTP(超文本传输协议)
    构建于TCP/IP协议之上，默认端口号是80，无连接无状态的。

    HTTP 请求报文包括 状态行、请求头、消息主体
    Content-Type：
      application/x-www-form-urlencoded：form表单
      multipart/form-data：表单上传文件
      application/json
      application/x-protobuf

    HTTP 响应报文
    200 OK 客户端请求成功
    3xx 重定向
    4xx 客户端错误
    5xx 服务器错误

    HTTP 1.1标准开始支持：
      Connection: Keep-Alive（持久连接）  

    如何防范 CSRF(跨站请求伪造) 攻击？
      1.验证码 2.检测 Referer
    如果防范 XSS(跨站脚本攻击) 攻击？
      1.过滤用户的输入
传输层 
  TCP(传输控制协议): 一个面向连接的、可靠的、基于字节流的传输层通信协议。
  UDP(用户数据报协议): 一个简单的面向数据报的传输层协议。
  TCP的数据单元称为段 （segments） UDP协议的数据单元称为数据报（datagrams）

  UDP报文的最大长度为512字节，而TCP则允许报文长度超过512字节。
  当DNS查询超过512字节时，协议的TC标志出现删除标志，这时则使用TCP发送。
  通常传统的UDP报文一般不会大于512字节。

  1.TCP面向连接; UDP是无连接的
  2.TCP提供可靠的服务,通过TCP连接传送的数据,无差错,不丢失,不重复,且按序到达。
    UDP尽最大努力交付，即不保证可靠交付。
  3.TCP面向字节流; UDP是面向报文的,UDP没有拥塞控制,
  4.每一条TCP连接只能是点到点的; UDP支持一对一，
    一对多，多对一和多对多的交互通信。
  5.TCP首部开销20字节; UDP的首部开销小,只有8个字节。
  6.TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道。

  TCP建立连接要进行3次握手,而断开连接要进行4次。

  建立连接：
  C -> S 1.请求建立连接
  C <- S 2.收到请求，确认建立连接，可以传输数据
  C -> S 3.确认应答，保证C也可以收到B的消息(可靠性)
  为什么要三次握手？防止了服务器端的一直等待而浪费资源。
  
  断开连接：
  C -> S 1.请求断开连接
  C <- S 2.收到请求(如果此时还有数据要传输呢？)
  C <- S 3.发送一个关闭请求(数据传完了！)
  C -> S 4.确认请求，连接正式关闭
  为什么要四次分手？确保未传输完的数据传输完毕。

网络层 IP 数据的单位称为数据包（packet）

数据链路层 数据的单位称为帧（frame）
物理层 数据的单位称为比特（bit）
```