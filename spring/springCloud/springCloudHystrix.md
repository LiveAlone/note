# spring cloud hystrix 

## 路由中断方式

Hystrix 服务之间的 网络调用， 不保证 100% 的可用性。
单个网络有问题，网络涌入，时间段形成任务的失败累计，导致服务的瘫痪， 甚至雪崩。
Hystrix 的实现， 5 秒 20 次的 调用， 断网路由会被打开

## hystrix 通过 Dashboard 方式，监控调用方式

1. 通过 ribbon, feign 均衡方式， Hystrix 方式， 作为中断器。

## 基础的实践
















