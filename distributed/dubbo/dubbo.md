#### Dubbo

```
名词：
  RPC(Remote Procedure Call Protocol)：远程过程调用协议
  SOA：面向服务的体系结构
  RMI(Remote Method Invocation)：远程方法调用
  SOAP(Simple Object Access Protocol)：简单对象访问协议
概念：Dubbo是一个分布式服务治理框架,致力于提供高性能透明化RPC远程调用方案,
  提供SOA服务治理解决方案。
核心模块：
  服务注册中心：zookeeper redis
  数据通信模块；netty mina
  数据序列化：hession2 hession java序列化
  RPC协议：dubbo(基于hessian2二进制序列化) rmi
深度分析：
  注册：dubbo multicast redis zookeeper
  协议：
    protocol: 
      dubbo rmi => TCP
      hessian webservice => HTTP
    序列化: hession2 dubbo java序列化 fst kryo fastjson   
  代理：jdk javassist
  请求拦截：filter过滤器
  容器：spring jetty spi
  通信：netty mina servlet
  事件处理：threadpool dispather
  容错：cluster:
    failover failfast failsafe failback
  路由：loadbalance 负载均衡策略

  默认依赖：spring容器 + javassist代理 + netty通信
  一般采用zookeeper服务注册中心
  序列化：
  RPC协议：
不同协议组合性能对比：
  RPC协议：连接方式+传输协议+通信框架+序列化方式
  dubbo：单一长连接+TCP+NIO异步传输(Netty/Mina)+Hessian二进制序列化
    适用于高并发小数据，消费者个数比提供者个数多
  rmi: 多个端连接+TCP+同步传输+Java标准二进制序列化
    适用于文件传输，消费者与提供者个数差不多。
  http: 多个短连接+HTTP+同步传输+Json表单序列化   
    采用Spring的HttpInvoker实现，适用于表单提交，提供者比消费者个数多
  hessian: 多个短连接+HTTP+同步传输+Hessian二进制序列化
    适合于文件传输，提供者比消费者个数多，提供者压力较大。
  webservice: 多个短连接+HTTP+同步传输+SOAP文本序列化  
    适合于系统集成，跨语言调用
```