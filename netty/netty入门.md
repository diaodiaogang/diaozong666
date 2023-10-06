# 二.Netty 入门

## 1.概述

### 1.1Nteey是什么？

Netty是一个异步，基于事件驱动的网络应用框架，用于

快速开发可维护、高新能的网络服务器和客户端。

### 1.2Netty 的优势

- 多路复用
- 解决粘包、半包
- 需要自己构建协议
- 对api进行增强

## 2.编写hello，word

**2.1需要一个客户端和服务端**

![image-20230917190104824](C:\Users\13038\AppData\Roaming\Typora\typora-user-images\image-20230917190104824.png)

**2.2服务端类**

```java
public class HelloServer {
    public static void main(String[] args) {
        //1.启动器，负责组装netty主件，启动服务器
        new ServerBootstrap()
        //2.分组
                .group(new NioEventLoopGroup())
        //3.选择服务器的ServerSocketChannel实现
                .channel(NioServerSocketChannel.class)
                //4.boss负责处理连接worker(child)负责处理编写，决定了woker(child)能执行那些操作(handler)
                .childHandler(
                        //5.channel代表和客户端进行数据读写通道Initiaalizer初始化，负责添加别的handler
                        new ChannelInitializer<NioSocketChannel>() {
                            @Override
                            protected void initChannel(NioSocketChannel ch) throws Exception {
                                //6.添加具体handler
                                ch.pipeline().addLast(new StringDecoder());//解码bytebuffer转换为字符串
                                ch.pipeline().addLast(new ChannelInboundHandlerAdapter() {//自定义handler
                                    @Override
                                    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
                                        System.out.println(msg);
                                    }
                                });
                            }
                        }
                )
                .bind(8080);
    }
}
```

**2.3客户端类**

```java
public class HelloClient {
    public static void main(String[] args) throws InterruptedException {
            //1. 启动类
                new Bootstrap()
                        //2.添加EventLoop
                        .group(new NioEventLoopGroup())
                        //3.选择客户端channel实现
                        .channel(NioSocketChannel.class)
                        //4.添加处理器
                        .handler(new ChannelInitializer<NioSocketChannel>() {
                            @Override //在连接建立后被调用
                            protected void initChannel(NioSocketChannel ch) throws Exception {
                                ch.pipeline().addLast(new StringEncoder());
                            }
                        })
                        //5.连接到服务器
                        .connect(new InetSocketAddress("localhost",8080))
                        .sync()
                        .channel()
                        //6.向服务器发送数据
                        .writeAndFlush("hello，word");
    }
}
```

**2.4运行**

![image-20230917190343433](C:\Users\13038\AppData\Roaming\Typora\typora-user-images\image-20230917190343609.png)
