# 从 Java BIO 到 NIO 再到多路复用


## IO 模型分类


### 分类

IO 模型分类：

1. __BIO (Blocking I/O)__：同步阻塞 I/O，传统的 I/O 模型。在进行 I/O 操作时，必须等待数据读取或写入完成后才能进行下一步操作。

2. __NIO (Non-Blocking I/O)__：同步非阻塞 I/O，是一种事件驱动的 I/O 模型。在进行 I/O 操作时，不需要等待操作完成，就可继续进行其他操作，

3. __AIO (Asynchronous I/O)__：异步非阻塞 I/O，是一种更高级别的 I/O 模型。在进行 I/O 模型时，不需要等待操作完成，就可继续进行其他操作，当操作完成后会自动回调通知。

由此可见，I/O 涉及四个非常重要的概念：

1. __同步__
2. __异步__
3. __阻塞__
4. __非阻塞__

我认为弄懂了这些概念才能深入了解 IO !


### 栗子

比如，中午吃饭时：

- __自选餐线__：我们点餐的时候都得在队伍里排队等待，必须等待前面的人打好菜才到我们，这就是 __同步阻塞模型 BIO__。

- __麻辣烫餐线__：会给我们发个叫号器，我们拿到叫号器后不需要排队原地等待，我们可以先找个地方去做其他事情，等麻辣烫准备好，我们收到呼叫之后，自行取餐，这就是 __同步非阻塞模型 AIO__。

- __包厢模式__：我们只要点好菜，坐在包厢可以自己玩，等到饭做好，服务员亲自送，无需自己取，这就是 __异步非阻塞 I/O 模型 AIO__。

### 概念详解

用上面的栗子和我们实际工作中的概念进行对比：

- 食堂 -> 操作系统

- 饭菜 -> 数据

- 饭菜好了 -> 数据就绪

- 端菜/送菜 -> 数据读取

#### 阻塞和非阻塞

__菜没好，是否原地等待 -> 数据就绪前是否等待__

- __阻塞__：数据为就绪，读阻塞知道有数据，缓冲区满时，写操作也会阻塞等待。本质上是线程挂起，不能做其他的事。

- __非阻塞__：请求直接返回，本质上线程活跃，可以处理其他事情。

#### 同步与异步

__菜好了，谁端？ -> 数据就绪，是操作系统送过去，还是应用程序自己读取__

- __同步__：数据就绪后，应用程序自己读取。

- __异步__：数据就绪后，操作系统直接回调应用程序。


#### Java 支持的版本

| 栗子          | IO 模型               | 支持的 JDK 版本                                                                       |
| ------------ | -------------          | ------------                                                                        |
| 排队打饭      | BIO（同步阻塞 I/O）    | JDK 1.4 之前                                                                         |
| 等待叫号      | NIO（同步非阻塞 I/O）  | JDK 1.4 之后（2002，`java.nio` 包）                                                     |
| 包厢模式      | AIO（异步非阻塞 I/O）  | JDK 1.7 之后（2011，`java.nio` 包下 `java.nio.channels.AsynchronousSocketChannel` 等）    |

其实，AIO 在很早就支持了，但是业界主流 IO 框架很少使用呢 ？比如 Netty，从 2.x 开始引入 AIO，随后，3.X 也继续保持，但到了 4.x 取删除对 AIO 的支持，能让一款世界级最优秀的 IO 框架之一做出舍弃 AIO，原因是：

- 对 AIO 支持最好的是 Windows 系统，但是很少用 Windows 做服务器。

- Linux 常用来做服务器，但 AIO 的实现不够成熟。

- Linux 下 AIO 相比 NIO 性能提升并不明显。

- 维护成本过高。

基于这些原因，下文就主要说说 BIO，NIO。


### 演练

#### c10k 问题

谈起 IO 模型的演进，不得不提 c10k 问题，[c10k（cuncurrent 10,000 connections）](http://www.kegel.com/c10k.html)。

接下来通过代码模拟 c10k 问题，然后通过使用不同 IO 模型来演进 IO 模型的发展历程。

> Talk is cheap. Show me the code.   - Linus Toiwalkd

#### 模拟 10,000 个客户端

直接用 BIO 串行创建 30000 个连接，代码如下：

```Java
public class C10kTestClient {

    static String ip = "192.168.110.155";

    public static void main(String[] args) throws IOException {
        LinkedList<SocketChannel> clients = new LinkedList<>();

        InetSocketAddress serverAddr = new InetSocketAddress(ip, 9998);

        IntStream.range(20000, 50000).forEach(i -> {
            try {
                SocketChannel client = SocketChannel.open();
                client.bind(new InetSocketAddress(ip, i));
                client.connect(serverAddr);
                System.out.println(STR."client: \{i} connected");

            } catch (IOException e) {
                System.out.println(STR."IOException: \{i}");
                e.printStackTrace();
            }
        });
        System.out.println(STR."client.size: \{clients.size()}");
        // 阻塞主线程
        System.in.read();
    }
}
```

#### BIO 服务端

代码如下：

```Java
public class BIOServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(9998, 20);
        System.out.println("BIO SERVER BEGIN");
        while (true) {
            // 阻塞 1
            Socket client = server.accept();
            System.out.println(STR."accept client: \{client.getPort()}");
            new Thread(() -> {
                try {
                    InputStream in = client.getInputStream();
                    BufferedReader reader = new BufferedReader(new InputStreamReader(in));
                    while (true) {
                        // 阻塞 2
                        String data = reader.readLine();
                        if (Objects.isNull(data)) {
                            System.out.println(data);
                        } else {
                            client.close();
                            break;
                        }
                    }
                    System.out.println("CLIENT BREAK");
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }).start();
        }
    }
}
```

上面代码第 7 行，因为接收到一个客户端连接后才能继续运行，所以会产生阻塞；第 16 行，`redaer.readline()` 也会产生阻塞。如果是单线程，则线程挂起，那么只能处理极少数连接。所以为了让程序能支持多个客户端，不得不使用多线程，最终得到模型如下图：

- [ ] TODO 插入一张图 ![]()

这段代码在 Windows 本地跑，报如下错误：

![](https://shichuan-hao.github.io/images/static/java/java-io-ck10-BIO.png)

在 Ubuntu 本地跑，报错如下：

![](https://shichuan-hao.github.io/images/static/java/java-io-ck10-BIO-ubuntu.png)

`too many open files`，这里说明打开的文件描述符过多。

!!! note annotate "什么是文件描述符 ？"

    在 Linux 中，一切皆文件。文件描述符（file descriptor，简称 fd）是一个索引值，指向一个文件记录表，该表记录内核为每一个进程维护的文件记录信息。

    查看 fd 信息，可以通过 `lsof -i -a -p [pid]` 查看当前进程打开的 tcp 相关的文件描述符。
 
由于在上面的代码中创建了 30,000 个 socket，而一个 socket（即一个 tcp 连接）就对应一个文件描述符（fd），30,000 个已经超过了系统默认的文件描述符限制。解决这个问题很简单参考 :material-door: <a href="https://shichuan-hao.github.io/boke/posts/java-net-SocketException-too-many-open-files/" target="_blank">__传送门 🚀__</a>

处理玩文件描述符过多问题后，继续重新跑遇到下面错误：

![](https://shichuan-hao.github.io/images/static/java/java-io-ck10-BIO-ubuntu-error.png)

这时的报错就跟在 Windows 上的报错就是一个意思了。

`unable to create new native thread` 和 `Address already in use: bind` 都是表明不能再创建新的本地线程。当然，系统线程限制是可以调节的。但是存在的问题也非常明显：

1. 一个连接需要一个线程，一台机器开辟线程数量有限。

2. 线程是轻量级进程，操作系统会为每一个线程分配 1M 独立的栈空间，如果要处理 c10k(10000 个连接)，就得有 10G 的栈空间。

3. 即便在内存空间充足的情况下，一台机器的 CPU 核数也是有限的。比如我们线上机器是 4 核，10000 个线程情况下，CPU 大量时间是耗费在线程调度而不是业务逻辑处理上，从而产生极大浪费。

因此 BIO 存在的核心问题是：<font color=red>组塞导致多线程</font>

!!! note annotate "如何解决 BIO 阻塞导致的多线程问题 ？"

    非阻塞 + 少量线程。



#### NIO 服务端

```Java
public class NIOServer {
    public static void main(String[] args) throws IOException, InterruptedException {
        LinkedList<SocketChannel> clients = new LinkedList<>();

        // 服务端开始监听
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.bind(new InetSocketAddress(9998));

        // 设置操作系统级别非阻塞 NONBLOCKING !!!
        serverSocketChannel.configureBlocking(false);

        while (true) {
            // 接受客户端的连接
            Thread.sleep(500);

            /**
             * accept 调用了内核
             * 在设置 configureBlocking(false) 及非阻塞的情况下：
             *  - 若有客户端连进来，直接返回客户端
             *  - 若无客户端连进来，则返回 null
             *  设置成 NONBLOCKING 后，代码不阻塞，线程不挂起，继续往下执行
             */
            SocketChannel client = serverSocketChannel.accept();

            if (Objects.isNull(client)) {
                System.out.println("null......");
            } else {
                // 重点，设置成 client 读写数据时阻塞
                client.configureBlocking(false);

                int port = client.socket().getPort();
                System.out.println(STR."client...port: \{port}");
                clients.add(client);
            }

            ByteBuffer buffer = ByteBuffer.allocateDirect(4096);
            // 遍历所有客户端，不需要多线程
            for (SocketChannel c : clients) {
                // 不阻塞
                int num = c.read(buffer);
                if (num > 0) {
                    buffer.flip();
                    byte[] bytes = new byte[buffer.limit()];
                    buffer.get(bytes);
                    String b = new String(bytes);
                    System.out.println(STR."\{c.socket().getPort() }: \{b}");
                    buffer.clear();
                }
            }
        }
    }
}
```

与 BIO 相比，NIO 有个特别牛逼的特性，就是通过 java.nio.channels.spi.AbstractSelectableChannel#configureBlocking 方法，调用内核，设置当前 socket 接收客户端连接，或者读取数据为非阻塞（BIO 中这两个操作都为阻塞）。


!!! note annotate "什么是 socket ？"

    Socket 是 TCP 连接的抽象，客户端 `client.connect(serverAddr)`；实际上底层就会调用系统内核处理三次握手，建立 tcp 连接。

从代码可以看出，相比 BIO，NIO 的优势：

- 建立连接和读写数据非阻塞

- 不需要开辟过多的线程

当然，NIO 也不是完美解决方案！仔细看上面代码 37 行到 48 行，就会发现，只要有一个连接进来，不管三七二十一就会遍历所有客户端，调用系统调用 `read` 方法。实际情况可能并没有客户端有数据达到，这就产生了一个新问题：<font color=red>无论是否有读写事件，都需要空遍历所有客户端连接，产生大量系统调用，大量浪费 CPU 资源。</font>


!!! note annotate "针对 NIO，无论是否有读写事件，都需要空遍历所有客户端连接，产生大量系统调用，大量浪费 CPU 资源，如何解决 ？"

    就是想办法，不去遍历所有客户端。因为 10,000 个客户端就需要调用 10,000 次系统调用，就会产生 10,000 次用户态和内核态的来回切换（回一下计算机组成原理，感受下这个资源消耗），而只调用一次内核就能知道哪些连接有数据。嗯，Linus Torvalds（Linux 之父）也是这样想的！所以就出现了<font color=red>多路复用</font>
    

## 多路复用

看下面代码：

```Java
public class SelectorNIOSimple {

    private Selector selector = null;

    public static void main(String[] args) {
        SelectorNIOSimple selectorNIOSimple = new SelectorNIOSimple();
        selectorNIOSimple.start();
    }


    @SuppressWarnings("InfiniteLoopStatement")
    public void start() {
        initServer();
        while (true) {
            Set<SelectionKey> keys = selector.keys();
            System.out.println(STR. "可处理事件数量 \{ keys.size() }" );
            try {
                if (!(selector.select() > 0)) {
                    // 返回的待处理的文件描述符集合
                    Set<SelectionKey> selectionKeys = selector.selectedKeys();
                    Iterator<SelectionKey> iterator = selectionKeys.iterator();
                    while (iterator.hasNext()) {
                        SelectionKey key = iterator.next();
                        // 使用后需要移除，否则会被一直处理
                        iterator.remove();
                        if (key.isAcceptable()) {
                            /**
                             * 对应系统调用
                             * select，poll 模式下：因为内核未开辟空间，那么在 JVM 中存放 fd4 的数组空间
                             * epoll 模式下：通过 epoll_ctl 把新客户端 fd 注册到内核空间
                             */
                            acceptHandler(key);
                        } else if (key.isReadable()) {
                            /**
                             * 处理读事件
                             */
                            readHandler(key);
                        }
                    }
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    private void readHandler(SelectionKey key) {
        try (SocketChannel client = (SocketChannel) key.channel()) {
            // 接受新客户端
            ByteBuffer buffer = (ByteBuffer) key.attachment();
            buffer.clear();
            // 重点，设置非阻塞
            int read;
            while (true) {
                read = client.read(buffer);
                if (read > 0) {
                    buffer.flip();
                    while (buffer.hasRemaining()) {
                        client.write(buffer);
                    }
                    buffer.clear();
                } else if (read == 0) {
                    break;
                } else {
                    client.close();
                    break;
                }
            }
        } catch (IOException e) {
            throw new RuntimeException();
        }
    }

    private void acceptHandler(SelectionKey key) {
        try (ServerSocketChannel ssc = (ServerSocketChannel) key.channel()) {
            SocketChannel client = ssc.accept();
            ;
            client.configureBlocking(false);
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            client.register(selector, SelectionKey.OP_READ, buffer);
            System.out.println("client connected: " + client.getRemoteAddress());
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private void initServer() {
        ServerSocketChannel server = null;
        try {
            server = ServerSocketChannel.open();
            server.configureBlocking(false);
            int port = 9998;
            server.bind(new InetSocketAddress(port));
            selector = Selector.open();
            server.register(selector, SelectionKey.OP_ACCEPT);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

如果对 多路复用 了解不多话，咋看起来比 BIO/AIO 代码更杂了！看下面.！

### 概念

!!! note annotate

    在 Linux 种， <font color=red>多路复用</font> 指的是一种实现同时监控多个文件描述符（包括 socket，文件和标准输入输出等）的技术。它可以通过一个进程同时接受多个连接请求或处理多个文件的 IO 操作，提供程序的效率和响应速度。

    结合上面栗子，这段话的意思就是 <font color=red>一次系统调用，就能多个客户端是否有读写事件</font>

    <font color=red>多路（多个客户端）复用（复用一次系统调用）</font>

    <font color=red>多路复用</font> 依赖内核的能力，不同的操作系统都有自己不同的 __多路复用器__ 实现，这里以 Linux 为例，<font color=red>多路复用</font> 分为两个阶段。


### 阶段一：select & poll

- select 是在 Linux 内核 2.0.0 版本中出现的，在 1996 年 6 月发布。

- poll 是在 Linux 内核 2.1.44 版本中出现的，于 1997 年 3 月发布。

在命令行输入 `man 2 select/poll` 查看 Linux 对它们的解释。

- [ ] TODO:补充 `man 2 select/poll` 图

对于 select 代码如下：
```C
int select(
    int nfds, // 要监视的文件描述符数量
    fd_set *restrict readfds, // 可读文件描述符集合
    fd_set *restrict writefds, // 可写文件描述符集合
    fd_set *restrict errorfds, // 异常文件描述符集合
)
```
在入参中，需要我们主动传入要监视的文件描述符数量、可读文件描述符集合、可写文件描述符集合以及异常文件描述符集合，实际上就干了一件事，以前有用户态去循环遍历所有客户端产生系统调用（如果 10K 个 socket，需要产生 10K 个系统调用），改成了由内核遍历，如果 select 模式，只需 10 个系统调用（因为 select 最大支持传入 1024 个文件描述符），如果是 poll 模式（不限制文件描述符数量），则只需 1 次系统调用。

poll 和 select 同属于第一阶段，因为它们处理问题的思路基本相同，区别如下：

1. 实现机制不同：select 使用轮询的方式来查询文件描述符上是否有事件发生，而 poll 则调用链表来存储文件描述符，查询时只需要对链表进行遍历。

2. 文件描述符的数量限制不同：select 最大支持 1024 个描述符，而 poll 没有数量限制，可以支持更多的文件描述符。

3. 阻塞方式不同：select 会阻塞整个进程，而 poll 可以只阻塞等待的文件描述符。

4. 可移植性不同：select 是 POSIX 标准中的函数，可以在各种操作系统上使用，而 poll 是 Linux 特有的函数，不是标准的 POSIX 标准中的函数，在其他操作系统上可能不被支持。

当然，select 和 poll 也不是很完美，依旧存在一个问题：<font color=red>大量的 fd（即连接）需要在用户态和内核态相互拷贝</font>。


???+ Tip "啥是高性能 ?"

    高性能首先需要做到的就是避免资源浪费，fd 集合在用户态和内核态互相拷贝就是一种浪费，越是在底层，一个细微的优化，对系统性能的提升都是巨大的。
    
    如何解决 ？Linus 大神又出手了，<font color=red>杜绝拷贝（不需要在用户态和内核态相互拷贝），空间换事件，在内核为应用程序开辟一块空间，这就是 epoll 要干的事情</font>。 


### 阶段二：epoll

- epoll 是在 Linux 内核 2.6.0 内核版本中发布，在 2002 年发布（java.nio 也刚好在 2002 年推出）

epoll 执行过程：

- [ ] TODO 补充 epoll 执行过程

1. 应用程序调用内核系统调用，开辟内核空间。对应的系统调用是 `int epoll_create(int size);`

2. 应用程序创建新连接，注册到对应的内核空间。对应的系统调用是 `int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);`

3. 应用程序询问需要处理的连接（哪些需要处理 ？有读、写、错误的事件）。`int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);`


### Java Selector

上面是 Linux 操作系统的 <font color=red>多路复用</font>的发展历程，java.nio 怎么使用呢 ？虽然在一开始就贴出来 Java Seclector 的代码实现，发现比前面的版本代码会复杂不少，但是第一遍只知道怎么写，至于为什么这么写，并不是很清楚，有了上面的铺垫，再看看这段代码：

```Java
public class SelectorNIOSimple {

    /**
     * Linux 多路复用器 默认使用 epoll.
     *
     * 通过启动参数指定使用 select poll 或者 epoll.
     */
    private Selector selector = null;

    public static void main(String[] args) {
        SelectorNIOSimple selectorNIOSimple = new SelectorNIOSimple();
        selectorNIOSimple.start();
    }

    private void initServer() {
        try {
            ServerSocketChannel server = ServerSocketChannel.open();
            server.configureBlocking(false);
            int port = 9998;
            server.bind(new InetSocketAddress(port));

            // epoll 模式下，open 会调用一个系统调用 epoll_create 返回文件描述符 fd3
            selector = Selector.open();

            /**
             * 对应系统调用
             * select, poll 模式下：JVM 里开辟一个文件描述符数组，并把 fd4 放入
             * epoll 模式下：调用内核 epoll_ctl(fd3, ADD, fd4, EPOLLIN)
             */
            server.register(selector, SelectionKey.OP_ACCEPT);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    @SuppressWarnings("InfiniteLoopStatement")
    public void start() {
        initServer();
        System.out.println("server start");
        while (true) {
            Set<SelectionKey> keys = selector.keys();
            System.out.println(STR. "可处理事件数量 \{ keys.size() }" );
            try {
                /**
                 * 对应系统调用
                 *
                 * 1. select, poll 模式下：调用内核 select(fd4) poll(fd4)
                 * 2. epoll 模式下：调用内核 epoll_wait();
                 */
                if (!(selector.select() > 0)) {
                    // 返回的待处理的文件描述符集合
                    Set<SelectionKey> selectionKeys = selector.selectedKeys();
                    Iterator<SelectionKey> iterator = selectionKeys.iterator();
                    while (iterator.hasNext()) {
                        SelectionKey key = iterator.next();
                        // 使用后需要移除，否则会被一直处理
                        iterator.remove();
                        if (key.isAcceptable()) {
                            /**
                             * 对应系统调用
                             * select, poll 模型下：因为内核未开辟空间，那么在 JVM 中存放 fd4 的数组空间
                             * epoll 模式下：通过 epoll_ctl 把新客户端 fd 注册到内核空间
                             */
                            acceptHandler(key);
                        } else if (key.isReadable()) {
                            /**
                             * 处理读事件
                             */
                            readHandler(key);
                        }
                    }
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    private void readHandler(SelectionKey key) {
        try (SocketChannel client = (SocketChannel) key.channel()) {
            ByteBuffer buffer = (ByteBuffer) key.attachment();
            buffer.clear();
            int read;
            while (true) {
                read = client.read(buffer);
                if (read > 0) {
                    buffer.flip();
                    while (buffer.hasRemaining()) {
                        client.write(buffer);
                    }
                    buffer.clear();
                } else if (read == 0) {
                    break;
                } else {
                    client.close();
                    break;
                }
            }
        } catch (IOException e) {
            throw new RuntimeException();
        }
    }

    private void acceptHandler(SelectionKey key) {
        try (ServerSocketChannel ssc = (ServerSocketChannel) key.channel()) {
            //接受新客户端
            SocketChannel client = ssc.accept();
            //重点，设置非阻塞
            client.configureBlocking(false);
            ByteBuffer buffer = ByteBuffer.allocate(1024);

            /**
             * 调用系统调用
             * select，poll模式下：jvm里开辟一个数组存入 fd7
             * epoll 模式下： 调用 epoll_ctl(fd3, ADD, fd7, EPOLLIN
             */
            client.register(selector, SelectionKey.OP_READ, buffer);
            System.out.println("client connected: " + client.getRemoteAddress());
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

- [ ] 补充

这里需要注意的是，如代码中注释所写，java 的 Selector 对所有多路复用器的一个抽象，可以通过系统属性设置多路复用器的类型。

具体来说，在启动 Java 应用程序时，通过 `-Djava.nio.channels.spi.SelectorProvider` 参加指定使用的 SelectorProvider 类，以此来设置多路复用器的类型。例如，使用以下命令启动 Java 应用程序。

```Shell
java -Djava.nio.channels.spi.SelectorProvider=sun.nio.ch.PollSelectorProvider MyApp
```

这个命令将使用 `PollSelectorProvider` 作为多路复用器的实现。如果不制定该参数，默认情况下将使用操作系统提供的默认多路复用实现，例如：

- Unix-like 系统中默认使用 EPollSelectorProvider。

- Windows 系统中默认使用 WindowsSelectorProvider。
