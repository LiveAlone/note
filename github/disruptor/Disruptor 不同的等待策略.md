# Disruptor 不同的等待策略

BlockingWaitingStrategy 阻塞方式， CPU 利用率较高。

- SleepingWaitStrategy ```LockSupport.parkNanos(1)``` 方法调用, 线程延时
  - 不需要低延迟的场景, 减少Producer 的影响， demo: 日志环境影响
- YieldingWaitStrategy 低延迟，忙等待的条件。 通过 ```Thread.yield()``` 方式， 让给其他线程
  - 使用场景： 高性能， 线程Handler 处理 < logical cores 小于核心线程数量, 
- BusySpinWaitStrategy 忙等待，最低响应延迟。
  - handler < physical cores 的数量

## handler 不会清楚 Event ， 手动清楚 Event 方式 