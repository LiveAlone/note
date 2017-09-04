# 一些重要的Es 配置

## 测试环境到生产环境, 添加Config 配置
- Jvm heap size
- 关闭 swapping
- 增加 File 文件描述
- 足够的 虚拟内存
- 足够的 线程数量

## 系统配置修改

- 线程数量限制修改 ulimit，```/etc/security/limits.conf``` 
- Service 限制 Limit Lock Memory 内存限制
- JVM Options

## Jvm heap的配置修改

- jvm.options 设置 Xms Xmx, 设置 Heap 的大小
- 配置相关规则
  - heap max,min 的大小相同
  - heap 越大， Es 可以缓存, GC 的时间较长。（设置 RAM 内存大小 50%, 足够的内存，kernal cache）
  - ```ES_JAVA_OPTS``` 配置， 设置Es JVM 的大小 

## 关闭 Swapping, Memory Disk 交换，会影响Es Node 的性能
- linux ```sudo swapoff -a``` ```/etc/fstab``` 
- ```vm.swappiness``` vm 交换, 系统Os 不会交换， 紧急还是会Swap
- bootstrap 启动check lock Es 级别， ```bootstrap.memory_lock: true```
- file 描述符数量 ```ulimit -n 65536``` 设置 ```/etc/security/limits.conf```
- Virtual Memory, ```vm.max_map_count``` setting in ```/etc/sysctl.conf```
  - To verify after rebooting, run sysctl ```vm.max_map_count```
- 线程数量 ```ulimit -u 2048```, ```/etc/security/limits.conf``` 设置线程数量
