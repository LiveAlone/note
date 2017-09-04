# java nio source

## byte

- ByteBuffer
  - capacity, limit, position 位置定义
  - clear 设置Buff 为可Channel 读状态
  - flip channel 写入状态，转换可读状态
  - rewind 重读方式
  - 线程不安全方式, 流方式编程
- Byte不同 Direct Heap 的 读写方式(Direct 较高的读写, 创建删除较高的代价)
  - HeapByteBuffer 内存拷贝 Jvm 的读写方式
  - MappedByteBuffer 基于 内存的数组映射方式
  - nio Direct 内存映射方式
    - 传统IO 读取方式, 磁盘 -> os 缓存内存 -> java process 内存
    - nio 磁盘文件直接映射到 用户私有地址空间的内存一部分
    - java 映射模式, 只读，读写，专用模式, 读写映射的文件，会立即被多个Process 查看到。
    - DirectMemory 并不受 JVM 堆大小的限制, 限制虚拟空间的大小
