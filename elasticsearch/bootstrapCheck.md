# Es bootstap check

## import settings

- path.data, path.logs 日志路径配置
- cluster name, node name 集群节点
- bootstrap.memory_lock
- network.host IP 绑定
- discovery.zen.ping.unicast.hosts, discovery.zen.minimum_master_nodes
  - 单节点, 绑定接口

## es 启动检测

- dev prd 不同启动模式
  - transport 绑定 External Interface
- 设置 Discovery.type=single-node 节点
- 设置启动```es.enforce.bootstrap.checks=true```,  强制启动节点配置
- heap size 检测
  - jvm init heap size 不等于 max heap size
  - bootstrap.memory_lock=true JVM会在启动时锁定堆的初始size
  - 果堆的初始size和最大size不同，resize 之后JVM的堆就不会锁定
- 文件 ulimit 限制配置文件数量的限制
- memory lock check, 磁盘， 内存的 交换的检测。
  - 即便打开了bootstrap.memory_lock，Elasticsearch仍有可能无法锁定堆
  - 系统设置 mlockall 设置
- 线程最大数量限制
  - Es 请求多线程的处理，可以创建 2048个线程数量
  - 修改/etc/security/limits.conf中的nproc即可调整该限制
- 虚拟内存 Size 设置
  - mmap将索引转换为地址空间, 确保Es 无限制的地址空间
  - 系统OS 虚拟内存的设置
  - /etc/security/limits.conf, nulimited 的 限制
- 最大映射条数
  - Es 需要内存映射区域
  - sysctl, vm.max_map_count 最大 262144
- jvm 检测
  - open-jdk: server jvm, client jvm
  - 检测启动的 服务端 jvm
- 系统调用过滤
  - Elasticsearch安装了很多系统调用过滤器，用来滤掉一些对Elasticsearch有害的fork调用。
