### 常用命令
#### 编译，运行，打包
1. sbt compile
2. sbt run
3. sbt package
#### 交互模式
> $ sbt
> \> compile
> \> run
> \> package
#### 可以一次运行多个命令
> \> clean compile 
#### SBT 常用命令
命令行文档：http://www.scala-sbt.org/release/docs/Command-Line-Reference.html
1. clean 删除 target 目录下面所有的文件
2. compile 编译 src/main/scala, src/main/java 和项目根目录下面的源文件
3. ~ compile 运行 sbt 交互模式时自动更新编译源码
4. console 编译项目中源码，并将它们加载到 classpath 中，并且启动REPL
5. doc 为使用 scaladoc 的 Scala 源码生成 API 文档
6. help <command>
7. inspect <setting>
8. package 生成 jar 文件， 包含 src/main/scala, src/main/java 和 src/main/resources 下的资源文件
9. package-doc 创建一个包含 scala 源码生成的 API 文档 和 jar 文件
10. publish 发布到远程仓库
11. pulish-local 将项目发布到本地的 Ivy 仓库
12. reload 重新加载构建定义文件（build.sbt, project/*.scala, project/*.sbt）
13. run 运行项目，如果项目中有多个 main, 需要按照提示选择执行
14. test 运行单元测试
15. update 更新外部依赖
16. 