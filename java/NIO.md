# NIO 库

[TOC]

## 1. Buffer

在没有使用nio之前，我们只能自己维护一个byte数组或者是char数组来进行批量读写，或者使用BufferedReader，BufferedInputStream来做读写缓冲。在nio里，就可以使用buffer了

### 1.1 缓冲区基础

本质上，缓冲区是就是一个数组。所有的缓冲区都具有四个属性来提供关于其所包含的数组的信息。它们是：

```java
  // Invariants: mark <= position <= limit <= capacity
    private int mark = -1;
    private int position = 0;
    private int limit;
    private int capacity;
```

1. 容量（Capacity） 缓冲区能够容纳的数据元素的最大数量。容量在缓冲区创建时被设定，并且永远不能被改变。
2. 上界（Limit） 缓冲区里的数据的总数，代表了当前缓冲区中一共有多少数据。
3. 位置（Position） 下一个要被读或写的元素的位置。Position会自动由相应的 get( )和 put( )函数更新。
4. 标记（Mark） 一个备忘位置。用于记录上一次读写的位置。一会儿，我会通过reset方法来说明这个属性的含义。

我们以字节缓冲区为例，ByteBuffer是一个抽象类，不能直接通过 new 语句来创建，只能通过一个static方法 allocate 来创建：

```java
ByteBuffer byteBuffer = ByteBuffer.allocate(256);
```

以上的语句可以创建一个大小为256字节的ByteBuffer，此时，mark = -1, pos = 0, limit = 256, capacity = 256。capacity在初始化的时候确定了，运行时就不会再变化了，而另外三个变量是随着程序的执行而不断变化的。

### 1.2 缓存区的存取

缓冲区用于存取的方法定义主要是put , get。

put 方法有多种重载，我们看其中一个：

```java
    public ByteBuffer put(byte x) {
        hb[ix(nextPutIndex())] = x;
        return this;
    }

    final int nextPutIndex() {                          // package-private
        if (position >= limit)
            throw new BufferOverflowException();
        return position++;
    }
```

这个方法是把一个byte变量 x 放到缓冲区中去。position会加1。再来看一下get方法，也是从position的位置去取缓冲区中的一个字节：

```java
    public byte get() {
        return hb[ix(nextGetIndex())];
    }

    final int nextGetIndex() {                          // package-private
        if (position >= limit)
            throw new BufferUnderflowException();
        return position++;
    }
```

那比如，我要是想在一个Buffer中放入了数据，然后想从中读取的话，就要把position调到我想读的那个位置才行。为此，ByteBuffer上定义了一个方法：

```java
    public final Buffer position(int newPosition) {
        if ((newPosition > limit) || (newPosition < 0))
            throw new IllegalArgumentException();
        position = newPosition;
        if (mark > position) mark = -1;
        return this;
    }
```

这里面用到了limit，想一下上面的定义，limit代表可写或者可读的总数。一个新创建的bytebuffer，它可写的总数就是它的capacity。如果写入了一些数据以后，想从头开始读的话，这时候的limit应该就是当前ByteBuffer中数据的总长度。下面的这个图比较直观地说明了这个问题：

![nio_1](..\img\nio_1.png)

为了达到从写数据的情况变成读数据的情况，还需要修改limit，这就要用到limit方法：

```java
    public final Buffer limit(int newLimit) {
        if ((newLimit > capacity) || (newLimit < 0))
            throw new IllegalArgumentException();
        limit = newLimit;
        if (position > limit) position = limit;
        if (mark > limit) mark = -1;
        return this;
    }
```

我们可以这样写，就把byteBuffer从读变成写了：

```java
byteBuffer.limit(byteBuffer.position())
byteBuffer.position(0);
```

当然，由于这个操作非常频繁，jdk就为我们封装了一个这样的方法，叫做flip:

```java
    public final Buffer flip() {
        limit = position;
        position = 0;
        mark = -1;
        return this;
    }
```

使用这个反转方法，思路一定要清晰，稍有不慎，就会带来莫名其妙的错误，比如，连续调用flip会对ByteBuffer有什么样的影响呢？这其实会使得Buffer的limit变成0，从而既不能读也不能写了。

limit的设计确实可以加速数据溢出情况的检查，但是造成使用上和理解上的困难，我还是觉得得不偿失。

### 1.3  缓冲区标记

在理解了position的作用以后，mark就很容易理解了，它就是记住当前的位置用的：

```java
    public final Buffer mark() {
        mark = position;
        return this;
    }
```

我们在调用过mark以后，再进行缓冲区的读写操作，position就会发生变化，为了再回到当初的位置，我们可以调用reset方法恢复position的值：

```java
    public final Buffer reset() {
        int m = mark;
        if (m < 0)
            throw new InvalidMarkException();
        position = m;
        return this;
    }
```

### 1.4 其他方法

clear:

```java
   /**
     * Clears this buffer.  The position is set to zero, the limit is set to
     * the capacity, and the mark is discarded.
     *
     * <p> Invoke this method before using a sequence of channel-read or
     * <i>put</i> operations to fill this buffer.  For example:
     *
     * <blockquote><pre>
     * buf.clear();     // Prepare buffer for reading
     * in.read(buf);    // Read data</pre></blockquote>
     *
     * <p> This method does not actually erase the data in the buffer, but it
     * is named as if it did because it will most often be used in situations
     * in which that might as well be the case. </p>
     *
     * @return  This buffer
     */
    public final Buffer clear() {
        position = 0;
        limit = capacity;
        mark = -1;
        return this;
    }

```

rewind:

```java
 /**
     * Rewinds this buffer.  The position is set to zero and the mark is
     * discarded.
     *
     * <p> Invoke this method before a sequence of channel-write or <i>get</i>
     * operations, assuming that the limit has already been set
     * appropriately.  For example:
     *
     * <blockquote><pre>
     * out.write(buf);    // Write remaining data
     * buf.rewind();      // Rewind buffer
     * buf.get(array);    // Copy data into array</pre></blockquote>
     *
     * @return  This buffer
     */
    public final Buffer rewind() {
        position = 0;
        mark = -1;
        return this;
    }
```

remaining:

```java
 /**
     * Returns the number of elements between the current position and the
     * limit. </p>
     *
     * @return  The number of elements remaining in this buffer
     */
    public final int remaining() {
        return limit - position;
    }
```

## 2. Channel

NIO中通过channel封装了对数据源的操作，通过 channel 我们可以操作数据源，但又不必关心数据源的具体物理结构。

这个数据源可能是多种的。比如，可以是文件，也可以是网络socket。在大多数应用中，channel 与文件描述符或者 socket 是一一对应的。

在Java IO中，基本上可以分为文件类和Stream类两大类。Channel 也相应地分为了FileChannel 和 Socket Channel，其中 socket channel 又分为三大类，一个是用于监听端口的ServerSocketChannel，第二类是用于TCP通信的SocketChannel，第三类是用于UDP通信的DatagramChannel。

### 1. Channel的作用

channel 最主要的作用还是用于非阻塞式读写。

Channel可以使用 ByteBuffer 进行读写，这是它的一个方便之处。我们先看一个例子，这个例子中，创建了一个ServerSocketChannel 并且在本机的8000端口上进行监听。

服务端：

```Java
public class WebServer {
    public static void main(String args[]) {
        try {
            ServerSocketChannel ssc = ServerSocketChannel.open();
            ssc.socket().bind(new InetSocketAddress("127.0.0.1", 8000));
            SocketChannel socketChannel = ssc.accept();

            ByteBuffer readBuffer = ByteBuffer.allocate(128);
            socketChannel.read(readBuffer);

            readBuffer.flip();
            while (readBuffer.hasRemaining()) {
                System.out.println((char)readBuffer.get());
            }

            socketChannel.close();
            ssc.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

用静态的 open( )工厂方法创建一个新的 ServerSocketChannel 对象，将会返回同一个未绑定的 java.net.ServerSocket 关联的通道。这个相关联的 ServerSocket 可以通过在 ServerSocketChannel 上调用 socket( )方法来获取。我们在这个例子中，使用了socket().bind来实现socket的绑定。

客户端代码：

```Java
public class WebClient {
    public static void main(String[] args) {
        SocketChannel socketChannel = null;
        try {
            socketChannel = SocketChannel.open();
            socketChannel.connect(new InetSocketAddress("127.0.0.1", 8000));

            ByteBuffer writeBuffer = ByteBuffer.allocate(128);
            writeBuffer.put("hello world".getBytes());

            writeBuffer.flip();
            socketChannel.write(writeBuffer);
            socketChannel.close();
        } catch (IOException e) {
        }
    }
}
```

新创建的 SocketChannel 虽已打开却是未连接的。在一个未连接的 SocketChannel 对象上尝试一 个 I/O 操作会导致 NotYetConnectedException 异常。我们可以通过在通道上直接调用 connect() 方法或在通道关联的 Socket 对象上调用 connect()来实现socket的连接。一旦一个 socket 通道被连接，它将保持连接状态直到被关闭。我们可以通过调用 isConnected()方法来测试某个 SocketChannel 是否已连接。 

结果：

```
h
e
l
l
o
 
w
o
r
l
d
```

就是说，我们把客户端发送过来的字符串逐字符地打印出来了。

同样可以使用InputStream和OutputStream进行字节流的读写，而且看起来，ByteBuffer似乎还没有直接使用byte[] 进行读写来得直观。实际上，channel 最大的作用并不仅限于此，它的最大作用是封装了异步操作，后面我会在 selector 的地方详细解释。

### 2. Scatter / Gather

Channel 提供了一种被称为 Scatter/Gather 的新功能，也称为本地矢量 I/O。Scatter/Gather 是指在多个缓冲区上实现一个简单的 I/O 操作。对于一个 write 操作而言，数据是从几个缓冲区（通常就是一个缓冲区数组）按顺序抽取（称为 gather）并使用 channel 发送出去。缓冲区本身并不需要具备这种 gather 的能力。gather 过程等效于全部缓冲区的内容被连结起来，并在发送数据前存放到一个大的缓冲区中。对于 read 操作而言，从 通道读取的数据会按顺序被散布（称为 scatter）到多个缓冲区，将每个缓冲区填满直至通道中的数据或者缓冲区的最大空间被消耗完。

大多数现代操作系统都支持本地矢量 I/O（native vectored I/O）。我们在一个通道上发起一个 Scatter/Gather 操作时，该请求会被翻译为适当的本地调用来直接填充或抽取缓冲区。这是一个很大的进步，因为减少或避免了缓冲区拷贝和系统调用。Scatter/Gather 应该使用直接的 ByteBuffers 以从本地 I/O 获取最大性能优势。

例如，我们可以把客户端改写成这个样子：

```java
public class WebClient {
    public static void main(String[] args) {
        SocketChannel socketChannel = null;
        try {
            socketChannel = SocketChannel.open();
            socketChannel.connect(new InetSocketAddress("127.0.0.1", 8000));

            ByteBuffer writeBuffer = ByteBuffer.allocate(128);
            ByteBuffer buffer2 = ByteBuffer.allocate(16);
            writeBuffer.put("hello ".getBytes());
            buffer2.put("world".getBytes());

            writeBuffer.flip();
            buffer2.flip();
            ByteBuffer[] bufferArray = {writeBuffer, buffer2};
            socketChannel.write(bufferArray);
            socketChannel.close();
        } catch (IOException e) {
        }
    }
}
```

这样就实现了一个Gather IO。Gather IO 在现在还看不出来有什么作用。我们后面会看到，在并发场景下，这是一个非常好用的特性。

把服务端改造成Scatter IO:

```java
public class WebServer {
    public static void main(String[] args) {
        try {
            ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
            serverSocketChannel.socket()
                    .bind(new InetSocketAddress("127.0.0.1", 8000));
            SocketChannel socketChannel = serverSocketChannel.accept();

            ByteBuffer readBuffer = ByteBuffer.allocate(128);
            ByteBuffer buffer2 = ByteBuffer.allocate(16);
            socketChannel.read(readBuffer);
            socketChannel.read(buffer2);

            readBuffer.flip();
            buffer2.flip();
            ByteBuffer[] bufferArray = {readBuffer, buffer2};

            for (ByteBuffer buffer:bufferArray){
                while (buffer.hasRemaining()) {
                    System.out.println((char) buffer.get());
                }
            }


            socketChannel.close();
            serverSocketChannel.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

## 3. IO模型

### 3.1 完全阻塞模型

![io model1](..\img\nio_2.png)

就是说，如果我客户端发起了connect请求，那么当前线程就会休眠，等待服务端响应完毕，返回消息，才会继续走下去。这种代码，我们已经写过了：

```
socketChannel = SocketChannel.open();
socketChannel.connect(new InetSocketAddress("192.168.0.13", 8000));
System.out.println("Hello");
```

你会看到，在connect没有完成之前，最后第三行的println根本不会执行。也就是说客户端会阻塞在这个地方。

就好比，我叫个外卖，然后我就去大门口傻等着，外卖不送到，我也什么都不做，就坐在门口打盹，直到外卖小哥过来，把我叫醒，我才拿着外卖回家去吃。这样做显然效率不高，显得脑子有水。

### 3.2 非阻塞式IO

把非阻塞的文件描述符称为非阻塞I/O。可以通过设置SOCK_NONBLOCK标记创建非阻塞的socket fd，或者使用fcntl将fd设置为非阻塞。

对非阻塞fd调用系统接口时，不需要等待事件发生而立即返回，事件没有发生，接口返回-1，此时需要通过errno的值来区分是否出错，有过网络编程的经验的应该都了解这点。不同的接口，立即返回时的errno值不尽相同，如，recv、send、accept errno通常被设置为EAGIN 或者EWOULDBLOCK，connect 则为EINPRO- GRESS 。

![io 2](..\img\nio_3.png)

就是说，客户端程序会不停地去尝试读取数据，但是不会阻塞在那个读方法里，如果读的时候，没有读到内容，也会立即返回。这就允许我们在客户端里，读到不数据的时候可以搞点其他的事情了。

仍然以外卖举例，就相当于，我一边扫地，一边等外卖。我不再像原来一样，在门口傻等了，而是扫两下，就跑到门口看看外卖到了没有。一直这样循环，直到我取到外卖，才从这个循环中跳出来，进入吃的流程。

### 3.3 IO多路复用

最常用的I/O事件通知机制就是I/O复用(I/O multiplexing)。Linux 环境中使用select/poll/epoll_wait 实现I/O复用，I/O复用接口本身是阻塞的，在应用程序中通过I/O复用接口向内核注册fd所关注的事件，当关注事件触发时，通过I/O复用接口的返回值通知到应用程序。I/O复用接口可以同时监听多个I/O事件以提高事件处理效率。

![io 3](..\img\nio_4.png)

还是外卖的例子，如果我们整栋楼的人，很多人叫了外卖，都有下楼来看外卖到没到的需求，于是物业就出了个招，让门卫小哥帮大家看着，整栋楼上的，不管是谁的外卖到了，先放到门卫小哥那里，然后门卫小哥再通知你下来拿自己的外卖。这样一来，我们就把本来多个人要跑去看自己的外卖到了这件事交给门卫小哥去做了。而我们解放出来，就可以继续看电视，打扫卫生。由于我们可以继续 做自己的事情，外卖小哥和门卫小哥在同时也在工作，互不干扰，所以这种工作方式就被称为**异步模型**

### 3.4 信号驱动(SIGIO)

除了I/O复用方式通知I/O事件，还可以通过SIGIO信号来通知I/O事件，如图所示。两者不同的是，在等待数据达到期间，I/O复用是会阻塞应用程序，而SIGIO方式是不会阻塞应用程序的。

![io 3](..\img\nio_5.png)

上面这张图，就是我们现实生活中真正的外卖。数据到达以后，给客户端发一个消息，让客户端过来取数据。这就像外卖小哥到你家门口给你打电话，让你出来取一下。显然，这种是最方便的，也是最合理的。

但实际上，在真正的编程中，我们很少使用这种模型。

### 3.5 sync io

POSIX规范定义了一组异步操作I/O的接口，不用关心fd 是阻塞还是非阻塞，异步I/O是由内核接管应用层对fd的I/O操作。异步I/O向应用层通知I/O操作完成的事件，这与前面介绍的I/O 复用模型、SIGIO模型通知事件就绪的方式明显不同。以aio_read 实现异步读取IO数据为例，如图所示，在等待I/O操作完成期间，不会阻塞应用程序.

![io 3](..\img\nio_6.png)

这个图，如果对应到外卖有点不合适了，比较像网购空调，我所要做的，只是下单，而快递小哥会把空调送过来，他也不会让你自己去取，他会让安装师傅直接帮你安装。这个过程中，你什么都不需要做。你所要做的仅仅是发起一个请求。这种IO模型就是纯正的异步IO。

这种纯异步IO的最典型例子就是node.js中的callback。

这5种IO并不是相互对立的，通过一定的技巧，是可以相互转化的。

## 4. 阻塞与非阻塞

阻塞IO：

```java
public class NetClient {
    public static void main(String[] args) {
        try {
            SocketChannel socketChannel = SocketChannel.open();
            socketChannel.connect(new InetSocketAddress("127.0.0.1", 8001));

            socketChannel.configureBlocking(true);

            ByteBuffer writeBuffer = ByteBuffer.allocate(1024);
            writeBuffer.put("hello world!".getBytes());
            writeBuffer.flip();

            socketChannel.write(writeBuffer);
            socketChannel.close();
        } catch (IOException e) {
        }
    }
}
```

非阻塞IO：

在Java中要使用非阻塞非常简单，只需要在socketChannel上调用：

```java
socketChannel.configureBlocking(false);
```

我们来看一下，它的具体实现：

在IDE里通过查看JDK源码可以找到：

```java
    protected void implConfigureBlocking(boolean var1) throws IOException {
        IOUtil.configureBlocking(this.fd, var1);
    } // in SocketChannelImpl
```

然后在IOUtil里看到这是一个 static native 方法：

```java
public static native void configureBlocking(FileDescriptor var0, boolean var1) throws IOException;
```

这个方法的具体实现位于 jdk/src/solaris/native/sun/nio/ch/IOUtil.c 中：

```c
static int 
configureBlocking(int fd, jboolean blocking)
{
    int flags = fcntl(fd, F_GETFL);
    int newflags = blocking ? (flags & ~O_NONBLOCK) : (flags | O_NONBLOCK);

    return (flags == newflags) ? 0 : fcntl(fd, F_SETFL, newflags);
}

JNIEXPORT void JNICALL
Java_sun_nio_ch_IOUtil_configureBlocking(JNIEnv *env, jclass clazz,
                                         jobject fdo, jboolean blocking)
{
    if (configureBlocking(fdval(env, fdo), blocking) < 0)
        JNU_ThrowIOExceptionWithLastError(env, "Configure blocking failed");
}
```

下面是configureBlocking的JNI定义，上面的那个是真正的实现。

## 5. Selector

在Java中，Selector这个类是select/epoll/poll的外包类，在不同的平台上，底层的实现可能有所不同，但其基本原理是一样的，其原理图如下所示：

![selector](..\img\nio_7.png)

所有的Channel都归Selector管理，这些channel中只要有至少一个有IO动作，就可以通过Selector.select方法检测到，并且使用selectedKeys得到这些有IO的channel，然后对它们调用相应的IO操作。

服务端：

```java
public class EpollServer {

    public static void main(String[] args) {
        try {
            ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
            serverSocketChannel.bind(new InetSocketAddress("127.0.0.1",8001));
            serverSocketChannel.configureBlocking(false);

            Selector selector = Selector.open();
            // 注册 channel，并且指定感兴趣的事件是 Accept
            serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

            ByteBuffer readBuff = ByteBuffer.allocate(1024);
            ByteBuffer writeBuff = ByteBuffer.allocate(128);
            writeBuff.put("received".getBytes());
            writeBuff.flip();

            while (true){
                int nReady = selector.select();
                Set<SelectionKey> keys = selector.selectedKeys();
                Iterator<SelectionKey> it = keys.iterator();

                while (it.hasNext()){
                    SelectionKey key = it.next();
                    it.remove();

                    if(key.isAcceptable()){
                        // 创建新的连接，并且把连接注册到selector上，而且，
                        // 声明这个channel只对读操作感兴趣。
                        SocketChannel socketChannel = serverSocketChannel.accept();
                        socketChannel.configureBlocking(false);
                        socketChannel.register(selector, SelectionKey.OP_READ);
                    }else if (key.isReadable()) {
                        SocketChannel socketChannel = (SocketChannel) key.channel();
                        readBuff.clear();
                        socketChannel.read(readBuff);

                        readBuff.flip();
                        System.out.println("received : " + new String(readBuff.array()));
                        key.interestOps(SelectionKey.OP_WRITE);
                    }
                    else if (key.isWritable()) {
                        writeBuff.rewind();
                        SocketChannel socketChannel = (SocketChannel) key.channel();
                        socketChannel.write(writeBuff);
                        key.interestOps(SelectionKey.OP_READ);
                    }
                }
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

1. 创建一个ServerSocketChannel，和一个Selector，并且把这个server channel 注册到 selector上，注册的时间指定，这个channel 所感觉兴趣的事件是 SelectionKey.OP_ACCEPT，这个事件代表的是有客户端发起TCP连接请求。
2. 使用 select 方法阻塞住线程，当select 返回的时候，线程被唤醒。再通过selectedKeys方法得到所有可用channel的集合。
3. 遍历这个集合，如果其中channel 上有连接到达，就接受新的连接，然后把这个新的连接也注册到selector中去。
4. 如果有channel是读，那就把数据读出来，并且把它感兴趣的事件改成写。如果是写，就把数据写出去，并且把感兴趣的事件改成读。

客户端：

```java
public class EpollClient {
    public static void main(String[] args) {
        try {
            SocketChannel socketChannel = SocketChannel.open();
            socketChannel.connect(new InetSocketAddress("127.0.0.1", 8001));

            ByteBuffer writeBuffer = ByteBuffer.allocate(32);
            ByteBuffer readBuffer = ByteBuffer.allocate(32);

            writeBuffer.put("hello".getBytes());
            writeBuffer.flip();

            while (true) {
                writeBuffer.rewind();
                socketChannel.write(writeBuffer);
                readBuffer.clear();
                socketChannel.read(readBuffer);
            }
        } catch (IOException e) {
        }
    }
}
```

## 6. 异步模型之状态机

同步（synchronization）是指在互斥的基础上（大多数情况），通过其它机制实现访问者对资源的有序访问。在大多数情况下，同步已经实现了互斥，特别是所有写入资源的情况必定是互斥的。少数情况是指可以允许多个访问者同时访问资源。它表达了任务间的直接制约关系，A要继续执行需要B完成某一个操作操作才能继续进行。

