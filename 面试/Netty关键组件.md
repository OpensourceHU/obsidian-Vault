

JavaNIO核心的三个类: Selector Channel Buffer

Netty可以说是在NIO的基础上, 配合Reactor模型 造出了Netty常用的类

| 组件模块                   | 说明                                                         |
| :------------------------- | :----------------------------------------------------------- |
| Boostrap & ServerBootstrap | Bootstrap 其实就是启动的意思，主要用来配置 Netty 的相关配置，串联各个组件，针对客户端。 |
| EventLoopGroup             | 是一组 EventLoop 的抽象，可以简单理解就是线程池，一般分为 BossEventLoopGroup 和 WorkerEventLoopGroup。 |
| ChannelFuture              | Netty 的所有 IO 操作都是异步的，通过注册监听器来监听执行结果的返回。 |
| Channel                    | Netty 的网络通信组件，客户端和服务端建立连接之后会维持一个 Channel。 |
| ChannelHandlerContext      | 保存 Channel 的上下文，同时关联一个 ChannelHandler 对象。    |
| ChannelHandler             | 自定义业务 Handler 需要实现它或它的子类，提供了一套生命周期的方法。 |
| ChannelPipeline            | ChannelPipeline 可认为是一个管道，是管理业务 Handler，通俗理解是保存 ChannelHandler 的 List 集合。 |
| ByteBuf                    | ByteBuf 是一个字节容器，提供了常见 api。Netty 是面向 ByteBuf 来传输数据的。 |
| 编码和解码                 | Netty 内置了常见编解码器，我们也可以自定义自己的编解码器。并且可以把编解码器封装成独立的 Handler，简化繁琐的流程。 |
| 拆包和粘包问题             | 了解为什么会出现拆包和粘包问题，如何去解决它，以及 Netty 内置了常见的拆包器。 |

