### 其它
#### 仓库
添加仓库：

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

#### 