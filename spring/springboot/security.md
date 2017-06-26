# security starter

## brief

- security 提供可 用户认证（Authentication）： 访问查看的权限。  用户授权（Authorization）：读写操作权限的确定
- 用户 basic user, manager, master. 认证方式， http http 表单 openId, LDAP, 提供了 ACL 的权限

## 基本用户认证授权

- ```UserDetailsService``` 通过username 获取 UserDetails， 获取用户的过期锁定的信息
- SecurityContext 通过 Name 获取配置权限信息， 设置线程继承，Global。 应对线程，请求用户的信息获取。
- 服务方法保护：Aop 实现方法级别的维护，用户权限不一致， AccessDeniedException
- 访问列表的控制， ACL 权限的分配配置。 配置用户的访问权限
  - concept
    - 授权主体 ACL_SID
    - 领域对象 进行控制访问的实体 Acl_class 实体对应的Java 名称 acl_object_identity 保存的实体本身
    - 访问权限： ACL_ENTRY 用户对象领域的权限访问
  - LADP
  - oauth