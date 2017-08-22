# 基础IO 相关的概念

## IO Java 演进
- java IO的缺点
  - 没有数据缓冲区, IO性能问题
  - 同步阻塞，线程的等待
- Linux 5中模型
  1. 阻塞IO 模型， 等待数据写入，阻塞，准备数据拷贝 
  2. 非阻塞IO, 循环是否准备完成
  3. IO 复用模型 Linux select/poll, epoll 事件驱动，性能效率较高
  4. 信号驱动模型
  5. 异步IO 模型， 系统调用， read, 拷贝用户空间, aio.read 读取指定的信号量
- java NIO 使用 IO 复用模型。
- io 多路复用技术， epoll 基于 select 改进
  - 进程打开的Socket 描述符的数量
  - IO 效率随着FD 的数量线性下降
  - mmap 内核用户空间 内存复制映射

## NIO 基础

- 传统BIO 模型
  - 通过单个 Accepter, 每个Browser, 不同的线程的链接方式
- 伪异步IO模型
  - 通过单个Accepter, 每个Brower，线程池满的时候， 会不执行
  - 弊端
    - 可能IO 阻塞的时间比较长
    - 线程的阻塞时间, 比较长，时间较长，会出现异常
    - 网络较慢的时候， 会产生多超时
- NIO 编程
  - ByteBuf nio 是直接，读取 写入缓冲区的数据， 不类似IO 拷贝 Stream
  - Channel, 是一个读写的双工通道
  - Selector 多路复用， 轮询Channel 方式
- AIO java 提供异步的IO 操作

## netty 的选择方式

- 人力，开发成的的使用， 较高的配置


