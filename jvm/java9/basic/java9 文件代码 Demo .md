# shejava9 文件Demo 配置方式

## Helloworld 过程

- 创建 module-info.java 的配置文件

  ```
  C:\Java9Revealed
  | --> C:\Java9Revealed\lib
  | --> C:\Java9Revealed\mods
  | --> C:\Java9Revealed\src
          | --> C:\Java9Revealed\src\com.jdojo.intros
  ```

  src 用来配置源码， mods 配置编译模块, lib 

- 创建一个模块```com.jdojo.intro``` 注意这是一个目录, 而不是一个子目录, 这里定义一个模块

- 指定文件， mods 编译生成class

  ```shell
   javac -d mods/org.yqj.demo org.yqj.demo/src/module-info.java org.yqj.demo/src/org/yqj/demo/Welcome.java
  ```

  ```--module-version 1.0``` 指定编译的版本 , 打包对应的模块指定的Modss

- 模块打包

  ```shell
  jar --create --file lib/org.yqj.demo-1.0.jar --main-class org.yqj.demo.Welcome -C mods/org.yqj.demo .
  ```

- 执行打包的模块

  ```shell
  java --module-path <module-path> --module <module>/<main-class>
  java --module-path lib --module org.yqj.demo/org.yqj.demo.Welcome
  ```

  指定模块执行Job

