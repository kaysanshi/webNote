## Netty

### Netty简介

Netty是由[JBOSS](https://baike.baidu.com/item/JBOSS)提供的一个[java开源](https://baike.baidu.com/item/java开源/10795649)框架，现为 [Github](https://baike.baidu.com/item/Github/10145341)上的独立项目。Netty提供异步的、[事件驱动](https://baike.baidu.com/item/事件驱动/9597519)的网络应用程序框架和工具，用以快速开发高性能、高可靠性的[网络服务器](https://baike.baidu.com/item/网络服务器/99096)和客户端程序。

Netty 是一个基于NIO的客户、服务器端的编程框架，使用Netty 可以确保你快速和简单的开发出一个网络应用，例如实现了某种协议的客户、[服务端](https://baike.baidu.com/item/服务端/6492316)应用。Netty的本质就是一个NIO框架，适用于服务器通讯相关的应用场景。Netty相当于简化和流线化了网络应用的编程开发过程，例如：基于TCP和UDP的socket服务开发。

- Netty提供了简单易用的API
- 基于事件驱动的编程方式来编写网络通信程序
- 更高的吞吐量
- 学习难度低。

### BIO,NIO,AIO

#### 概念

Java共支持3种网络编程模型/IO模式：BIO、NIO、AIO。

**Java BIO ：** **同步并阻塞(传统阻塞型)**，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销 【简单示意图】

**Java NIO ： 同步非阻塞**，服务器实现模式为一个线程处理多个请求(连接)，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求就进行处理 【简单示意图】

**Java AIO(NIO.2) ： 异步非阻塞**，AIO 引入异步通道的概念，采用了 Proactor 模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用

#### BIO

BIO是传统的同步阻塞性IO模型。其相关的类就是在java.io包下。服务端实现就是一个连接启动一个线程。每一个客户端的请求服务端都会创建一个线程的。BIO方式适用于连接数目比较小且固定的架构

##### BIO的工作机制以及流程

[![6M9x61.png](https://s3.ax1x.com/2021/03/07/6M9x61.png)](https://imgtu.com/i/6M9x61)

**BIO的编程的简单的流程**

1. 服务器端启动一个ServerSocket
2. 客户端启动Socket对服务器进行通信，默认情况下服务器端需要对每个客户 建立一个线程与之通讯
3. 客户端发出请求后, 先咨询服务器是否有线程响应，如果没有则会等待，或者被拒绝
4. 如果有响应，客户端线程会等待请求结束后，在继续执行

##### BIO的问题

- 每个请求都需要创建独立的线程，与对应的客户端进行数据 Read，业务处理，数据 Write 。
- 当并发数较大时，需要创建大量线程来处理连接，系统资源占用较大。
- 连接建立后，如果当前线程暂时没有数据可读，则线程就阻塞在 Read 操作上，造成线程资源浪费

#### NIO

NIO:java non-blocking IO 是指的JDK提供的新的API.其相关类是在java.nio包及子包内，对java.io进行了改写。

NIO是面向缓冲区的，或者面向块的编程的，数据读取到一个缓冲区内，需要时可在缓冲区内前后的移动，这就是增加了处理过程的灵活性，使用它可以提供非阻塞式的高伸缩性网络。

NIO的三大核心为：**Channel(通道)，Buffer(缓冲区)，Selector(选择器)。**

Java NIO的非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取，而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。

通俗理解：NIO是可以做到用一个线程来处理多个操作的。假设有10000个请求过来,根据实际情况，可以分配50或者100个线程来处理。不像之前的阻塞IO那样，非得分配10000个。

HTTP2.0使用了多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比HTTP1.1大了好几个数量级。

下面的一张图说明了三大核心的关系图：

[![6MikQA.png](https://s3.ax1x.com/2021/03/07/6MikQA.png)](https://imgtu.com/i/6MikQA)

**上面的关系图说明：**

1. 每个channel 都会对应一个Buffer
2. Selector 对应一个线程， 一个线程对应多个channel(连接)
3. 该图反应了有三个channel 注册到 该selector //程序
4. 程序切换到哪个channel 是有事件决定的, Event 就是一个重要的概念
5. Selector 会根据不同的事件，在各个通道上切换
6. Buffer 就是一个内存块 ， 底层是有一个数组
7. 数据的读取写入是通过Buffer, 这个和BIO , BIO 中要么是输入流，或者是输出流, 不能双向，但是NIO的Buffer 是可以读也可以写, 需要 flip 方法切换
8. channel 是双向的, 可以返回底层操作系统的情况, 比如Linux ， 底层的操作系统通道就是双向的.



##### Buffer

缓冲区（Buffer）：缓冲区本质上是一个可以读写数据的内存块，可以理解成是一个容器对象(含数组)，该对象提供了一组方法，可以更轻松地使用内存块，，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。Channel 提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由 Buffer

**常用Buffer子类一览**

[![6Mk0q1.png](https://s3.ax1x.com/2021/03/07/6Mk0q1.png)](https://imgtu.com/i/6Mk0q1)

- ByteBuffer，存储字节数据到缓冲区
- ShortBuffer，存储字符串数据到缓冲区
- CharBuffer，存储字符数据到缓冲区
- IntBuffer，存储整数数据到缓冲区
- LongBuffer，存储长整型数据到缓冲区
- DoubleBuffer，存储小数到缓冲区
- FloatBuffer，存储小数到缓冲区

```java
public abstract class Buffer {
// Invariants: mark <= position <= limit <= capacity
    // 标记
    private int mark = -1;
    // 位置，下一个要被读或者写的元素的索引，每次读写缓冲区数据时都会改变值，为下一次读写准备
    private int position = 0;
    // 表示缓冲区的当前终点，不能对缓冲区超过极限的位置进行读写操作。且极限是可以修改的
    private int limit;
    // 容量，即可以容纳的最大数据量；在缓冲区创建时被设定并且不能改变
    private int capacity;
}
```

###### ByteBuffer类

从前面可以看出对于 Java 中的基本数据类型(boolean除外)，都有一个 Buffer 类型与之相对应，最常用的自然是ByteBuffer 类（二进制数据），该类的主要方法如下：

```java
public abstract class ByteBuffer {
    //缓冲区创建相关api
    public static ByteBuffer allocateDirect(int capacity)//创建直接缓冲区
    public static ByteBuffer allocate(int capacity)//设置缓冲区的初始容量
    public static ByteBuffer wrap(byte[] array)//把一个数组放到缓冲区中使用
    //构造初始化位置offset和上界length的缓冲区
    public static ByteBuffer wrap(byte[] array,int offset, int length)
     //缓存区存取相关API
    public abstract byte get( );//从当前位置position上get，get之后，position会自动+1
    public abstract byte get (int index);//从绝对位置get
    public abstract ByteBuffer put (byte b);//从当前位置上添加，put之后，position会自动+1
    public abstract ByteBuffer put (int index, byte b);//从绝对位置上put
 }

```

##### Channel

NIO的通道类似于流，但有些区别如下：

- 通道可以同时进行读写，而流只能读或者只能写
- 通道可以实现异步读写数据
- 通道可以从缓冲读数据，也可以写数据到缓冲: 

BIO 中的 stream 是单向的，例如 FileInputStream 对象只能进行读取数据的操作，而 NIO 中的通道(Channel)是双向的，可以读操作，也可以写操作。

Channel在NIO中是一个接口public interface Channel extends Closeable{} 

常用的 Channel 类有：FileChannel、DatagramChannel、ServerSocketChannel 和 SocketChannel。【ServerSocketChanne 类似 ServerSocket , SocketChannel 类似 Socket】

FileChannel 用于文件的数据读写，DatagramChannel 用于 UDP 的数据读写，ServerSocketChannel 和 SocketChannel 用于 TCP 的数据读写。

###### FileChannel类

FileChannel主要用来对本地文件进行 IO 操作，常见的方法有

```java
//从通道读取数据并放到缓冲区中
public int read(ByteBuffer dst) ，

//把缓冲区的数据写到通道中
public int write(ByteBuffer src) ，

//从目标通道中复制数据到当前通道
public long transferFrom(ReadableByteChannel src, long position, long count)，

//把数据从当前通道复制给目标通道
public long transferTo(long position, long count, WritableByteChannel target)，
```

**关于Buffer 和 Channel的注意事项和细节**


ByteBuffer 支持类型化的put 和 get, put 放入的是什么数据类型，get就应该使用相应的数据类型来取出，否则可能有 BufferUnderflowException 异常。

可以将一个普通Buffer 转成只读Buffer

NIO 还提供了 MappedByteBuffer， 可以让文件直接在内存（堆外的内存）中进行修改， 而如何同步到文件由NIO 来完成.

前面我们讲的读写操作，都是通过一个Buffer 完成的，NIO 还支持 通过多个Buffer (即 Buffer 数组) 完成读写操作，即 Scattering 和 Gathering 

##### Selector

Java 的 NIO，用非阻塞的 IO 方式。可以用一个线程，处理多个的客户端连接，就会使用到Selector(选择器)

Selector 能够检测多个注册的通道上是否有事件发生(注意:多个Channel以事件的方式可以注册到同一个Selector)，如果有事件发生，便获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求。


只有在 连接/通道 真正有读写事件发生时，才会进行读写，就大大地减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程

避免了多线程之间的上下文切换导致的开销。

[![6M8tJJ.png](https://s3.ax1x.com/2021/03/07/6M8tJJ.png)](https://imgtu.com/i/6M8tJJ)

```tex
Netty 的 IO 线程 NioEventLoop 聚合了 Selector(选择器，也叫多路复用器)，可以同时并发处理成百上千个客户端连接。

当线程从某客户端 Socket 通道进行读写数据时，若没有数据可用时，该线程可以进行其他任务。

线程通常将非阻塞 IO 的空闲时间用于在其他通道上执行 IO 操作，所以单独的线程可以管理多个输入和输出通道。

由于读写操作都是非阻塞的，这就可以充分提升 IO 线程的运行效率，避免由于频繁 I/O 阻塞导致的线程挂起。

一个 I/O 线程可以并发处理 N 个客户端连接和读写操作，这从根本上解决了传统同步阻塞 I/O 一连接一线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升。

```

##### NIO非阻塞 网络编程原理分析图

[![6MJiE8.png](https://s3.ax1x.com/2021/03/07/6MJiE8.png)](https://imgtu.com/i/6MJiE8)

- 当客户端连接时，会通过ServerSocketChannel 得到 SocketChannel
- Selector 进行监听  select 方法, 返回有事件发生的通道的个数.
- 将socketChannel注册到Selector上, register(Selector sel, int ops), 一个selector上可以注册多个SocketChannel
- 注册后返回一个 SelectionKey, 会和该Selector 关联(集合)
- 进一步得到各个 SelectionKey (有事件发生)
- 在通过 SelectionKey  反向获取 SocketChannel , 方法 channel()
  可以通过  得到的 channel  , 完成业务处理。

###### SelectionKey

SelectionKey，表示 Selector 和网络通道的注册关系, 共四种:

int OP_ACCEPT：有新的网络连接可以 accept，值为 16
int OP_CONNECT：代表连接已经建立，值为 8
int OP_READ：代表读操作，值为 1 
int OP_WRITE：代表写操作，值为 4
源码中：

```java
public static final int OP_READ = 1 << 0; 
public static final int OP_WRITE = 1 << 2;
public static final int OP_CONNECT = 1 << 3;
public static final int OP_ACCEPT = 1 << 4;

public abstract class SelectionKey {
	public abstract Selector selector();//得到与之关联的 Selector 对象
	public abstract SelectableChannel channel();//得到与之关联的通道
	public final Object attachment();//得到与之关联的共享数据
	public abstract SelectionKey interestOps(int ops);//设置或改变监听事件
	public final boolean isAcceptable();//是否可以 accept
	public final boolean isReadable();//是否可以读
	public final boolean isWritable();//是否可以写
}

```

###### ServerSocketChannel

ServerSocketChannel 用于在服务器端监听新的客户端 Socket 连接

```java
public abstract class ServerSocketChannel extends AbstractSelectableChannel implements NetworkChannel{
	public static ServerSocketChannel open()，//得到一个 ServerSocketChannel 通道
	public final ServerSocketChannel bind(SocketAddress local)，//设置服务器端端口号
	public final SelectableChannel configureBlocking(boolean block)，//设置阻塞或非阻塞模式，取值 false 表示采用非阻塞模式
	public SocketChannel accept()，//接受一个连接，返回代表这个连接的通道对象
	public final SelectionKey register(Selector sel, int ops)，//注册一个选择器并设置监听事件
}

```

###### SocketChannel

SocketChannel，网络 IO 通道，具体负责进行读写操作。NIO 把缓冲区的数据写入通道，或者把通道里的数据读到缓冲区。

```java
public abstract class SocketChannel extends AbstractSelectableChannel  implements ByteChannel, ScatteringByteChannel, GatheringByteChannel, NetworkChannel{
	public static SocketChannel open();//得到一个 SocketChannel 通道
	public final SelectableChannel configureBlocking(boolean block);//设置阻塞或非阻塞模式，取值 false 表示采用非阻塞模式
	public boolean connect(SocketAddress remote);//连接服务器
	public boolean finishConnect();//如果上面的方法连接失败，接下来就要通过该方法完成连接操作
	public int write(ByteBuffer src);//往通道里写数据
	public int read(ByteBuffer dst);//从通道里读数据
	public final SelectionKey register(Selector sel, int ops, Object att);//注册一个选择器并设置监听事件，最后一个参数可以设置共享数据
	public final void close();//关闭通道
}

```

##### NIO和BIO对比

- BIO 以流的方式处理数据,而 NIO 以块的方式处理数据,块 I/O 的效率比流 I/O 高很多
- BIO 是阻塞的，NIO 则是非阻塞的
- BIO基于字节流和字符流进行操作，而 NIO 基于 Channel(通道)和 Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择器)用于监听多个通道的事件（比如：连接请求，数据到达等），因此使用单个线程就可以监听多个客户端通道 

##### NIO与零拷贝

#### AIO

JDK 7 引入了 Asynchronous I/O，即 AIO。在进行 I/O 编程中，常用到两种模式：Reactor和 Proactor。Java 的 NIO 就是 Reactor，当有事件触发时，服务器端得到通知，进行相应的处理

AIO 即 NIO2.0，叫做异步不阻塞的 IO。AIO 引入异步通道的概念，采用了 Proactor 模式，简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用

目前 AIO 还没有广泛应用，Netty 也是基于NIO, 而不是AIO， 因此我们就不详解AIO了

#### BIO,NIO,AIO对比

|          | BIO      | NIO        | AIO        |
| -------- | -------- | ---------- | ---------- |
| IO模型   | 同步阻塞 | 同步非阻塞 | 异步非阻塞 |
| 编程难度 | 简单     | 复杂       | 复杂       |
| 可靠性   | 差       | 好         | 好         |
| 吞吐量   | 低       | 高         | 高         |

### Netty高性能架构设计

