#### Java NIO
```
Channels(通道)
Buffers(缓冲区)
Selectors(选择器)

数据可以从Channel读到Buffer中，也可以从Buffer 写到Channel中。
Selector允许单线程处理多个 Channel。
如果你的应用打开了多个连接（通道），但每个连接的流量都很低，使用Selector就会很方便。单一长连接。
一个线程使用一个Selector控制三个Channel。
```

##### Channel
```
磁盘IO
  FileChannel
网络IO
  SocketChannel
  ServerSocketChannel
  DatagramChannel

方法介绍：
int bytesRead = inChannel.read(buf);
int bytesWritten = inChannel.write(buf);
  
FileChannel无法设置为非阻塞模式，它总是运行在阻塞模式下。  
```

##### Buffer
```
缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。

capacity 初始化大小
position 写=实际写入位置 读=可读位置从0开始 实际读取位置
limit 写=capacity 读=写入的position

ByteBuffer

ShortBuffer
IntBuffer
LongBuffer

FloatBuffer
DoubleBuffer

CharBuffer

MappedByteBuffer

方法介绍：
1.初始化一个capacity=?的Buffer
ByteBuffer buf = ByteBuffer.allocate(48);
CharBuffer buf = CharBuffer.allocate(1024);
2.往Buffer写入数据
从Channel写到Buffer
  int bytesRead = inChannel.read(buf);
通过Buffer的put()方法写到Buffer
  buf.put(127);
3.切换模式
  buf.flip();//写模式切换到读模式
4.从Buffer读取数据
从Buffer读取数据到Channel
  int bytesWritten = inChannel.write(buf);
使用get()方法从Buffer中读取数据     
  byte aByte = buf.get();
5.clear()与compact()方法
  claer()：
    position将被设回0，limit被设置成 capacity的值
  如果Buffer中有一些未读的数据，调用clear()方法，数据将“被遗忘”
  compact()：
    position设到最后一个未读元素正后面，limit被设置成 capacity的值
```

##### Scatter(分散)/Gather(聚集)
```
Scatter(分散) 一个Channel -> 被读取到 多个Buffer
Gather(聚集) 一个Channel <- 被写入从 多个Buffer
```

##### Channel之间的数据传输
```
toChannel.transferFrom(fromChannel, position, count);
fromChannel.transferTo(position, count, toChannel);
```

##### Selector
```
一个单独的线程可以管理多个channel，从而管理多个网络连接。

```