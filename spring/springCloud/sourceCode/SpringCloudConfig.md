# spring cloud config 配置

配置Server Client 相同，可以和不同的语言使用。

## spring cloud config 服务器配置

- 文件命名规则
  - {application} spring.application.name
  - {profile} spring.profiles.active
  - {label} 版本集的配置
- git backand 方式
  - EnvironmentRepository 实现，通过 Application,Profile,Label 获取Envoirment
- 支持符合环境
  - git svn, file 不同的环境
- 配置快速失败 ```spring.cloud.config.failFast=true```

