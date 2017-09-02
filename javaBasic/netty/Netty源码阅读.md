# netty 源码阅读

## demo discard

- ChannelInboundHandlerAdapter Handler实现Data Recevie
  - Handler 去Release 释放Channel
- NioEventLoopGroup 多线程 Event Loop 处理IO操作。
  - boss 负责Accept 输入连接， worker： 负责处理Boss 接受的Connection, 注册到worker, 
  - 线程处理数量， 映射对应Channel, 依靠 EventLoopGroup 对应的实现方式
- ServerBootStrap 帮助启动Server
  - NioServerScocketChannel 启动instance, 接受链接的Channel
  - ChannelInitializer 添加 Channel Handler 处理接受数据
  - option 添加参数， 例如 TCP/IP keepLive 等配置
  - childOption 设置 ParentChannel 接受的数据
- ChannelInBoundHandlerAdapter
  - channelRead 数据读取
  - channelActive 建立链接Handler
  

