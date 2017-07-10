# spring Cloud sleuth
集成 zipkin 方式，网状的复杂配置结构。

### 基本的术语配置
1. Span 工作单元，发送RPC 请求。 Span 唯一的 UUID 标识。
  - trace 通过 摘要，时间戳，Tags, ip 等。
1. Trace , 系列的 Spans 的一个树状结构，一个 Application 创建。
1. Annotation 的 开始结束
  - cs client send, 客户端发送请求
  - sr Server Received, 服务端接受处理请求， sr - cs = 网络延迟
  - ss Server Sent, 客户端返回的处理请求时间
  - cr Client Receiver 客户端接受请求的过程