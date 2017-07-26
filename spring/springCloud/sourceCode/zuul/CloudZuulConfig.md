# Cloud Zuul Base

## 基础路由过滤方式

- 过滤方式
  - 过程进行干预，是实现请求校验、服务聚合等功能
- 路径与配置的路由规则进行匹配

## 过滤器

- filterType:String 返回过滤的类型
  - zuul四生命周期
    - pre 路由之前调用
    - routing 路由被调用
    - post routing和error过滤器之后被调用
    - error 错误调用
  - filterOrder int 过滤器顺序
  - shouldFilter:Boolean 是否过滤
  - run 具体的执行逻辑

![执行逻辑](http://blog.didispace.com/content/images/2016/07/687474703a2f2f6e6574666c69782e6769746875622e696f2f7a75756c2f696d616765732f7a75756c2d726571756573742d6c6966656379636c652e706e67.png)

- Pre 请求与执行, routing 调用Server 服务, PostFilters 内部请求完成

## 核心过处理器

- spring-cloud-netflix-core 模块, org.springframework.cloud.netflix.zuul.filters 包
- pre 过滤器
  - ServletDetectionFilter 检验 Spring的DispatcherServlet 还是 ZuulServlet 处理
    - 通过 RequestUtils.isDispatcherServletRequest() 和 RequestUtils.isZuulServletRequest() 判断
    - 一般情况下，发送到API网关的外部请求都会被Spring的DispatcherServlet处理，除了通过/zuul/路径访问的请求会绕过DispatcherServlet，被ZuulServlet处理，主要用来应对处理大文件上传的情况。另外，对于ZuulServlet的访问路径/zuul/，我们可以通过zuul.servletPath参数来进行修改
  - Servlet30WrapperFilter
    - HttpServletRequest包装成Servlet30RequestWrapper对象 
  - FormBodyWrapperFilter
    - Content-Type为application/x-www-form-urlencoded的请求
    - Content-Type为multipart/form-data并且是由Spring的DispatcherServlet处理的请求
  - DebugFilter debug 对应信息
  - PreDecorationFilter forward.to和serviceId 判断是否存在
    - 可以设置 重定向，zuul 拦截操作
    - [定向zuul](http://blog.didispace.com/spring-cloud-zuul-cookie-redirect/)
- route 过滤器
  - RibbonRoutingFilter
      - Request 中包含 Service-Id 可以 Ribbon 区请求处理
      - ribbong 或者 hystrix 服务发送请求
      - 并且返回结果
  - SimpleHostRoutingFilter 
    - 上下文存在的 routHost 参数请求
    - 通过路由匹配规则，物理请求对应的机器
    - 没有Hystrix命令包装，请求并没有线程隔离和断路器的保护
  - SendForwardFilter 
    - forward.to 处理请求
    - 确认 地址跳转 方式
- Post 过滤器
  - SendErrorFilter： 上下文 error.status_code 参数，错误信息 组织 forward,/error 响应
  - SendResponseFilter
    - 没有 Error 信息
    - 设置 Header, ResponseBody 对应的 InputStream 输出流信息

![pic](http://blog.didispace.com/assets/zuul-filter-core.png)
