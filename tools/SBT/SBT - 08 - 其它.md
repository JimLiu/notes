### 其它
#### 仓库
添加仓库：

	// https://www.cnblogs.com/codingexperience/p/5372617.html
	resolvers += "Java.net Maven2 Repository" at "http://download.java.net/maven/2/"
用 Seq 添加多个 resolvers:
	
	resolvers ++= Seq(
		"Typesafe" at "http://repo.typesafe.com/typesafe/releases/",
		"Java.net Maven2 Repository" at "http://download.java.net/maven/2/"
	)

也可以写多行：

	resolvers += "Java.net Maven2 Repository" at "http://download.java.net/maven/2/"

	resolvers += "Java.net Maven2 Repository" at "http://repo.typesafe.com/typesafe/releases/"

#### 日志级别

在 build.sbt 中设置 SBT 日志等级：

	logLevel := Level.Debug //Level.Info, Warning, Error

#### 部署 jar
	
##### 使用 sbt-assembly
github: https://github.com/sbt/sbt-assembly
在 project 目录下面的 plugins.sbt 中添加下面两行代码:
	
	resolvers += Resolver.url("artifactory", url("http://scalasbt.artifactoryonline.com/scalasbt/sbt-plugin-releases"))(Resolver.ivyStylePatterns)
	
	addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.8.4")

然后将下面两行加在 build.sbt 文件开头：

	import AssemblyKeys._
	
	assemlySettings

运行：
	$ sbt assembly	





