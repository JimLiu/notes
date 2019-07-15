### 依赖管理
	可管理依赖与不可管理依赖
	libraryDependencies += groupID % artifactID % version [% configuration]
	version: 可以是 latest.integration, latest.milestone
#### 不可管理依赖
	将相应的 jar 包放在 lib/ 目录下面
#### 可管理依赖
	在 build.sbt 中 添加一行 libraryDependencies: libraryDependencies += "net.sourceforge.htmlcleaner" % "htmlcleaner" % "2.4"

因为 build.sbt 的配置行必须用空行隔开，在项目中添加多个依赖：

	libraryDependencies ++= Seq(
		"net.sourceforge.htmlcleaner" % "htmlcleaner" % "2.4",
		"xxx" % "yyy" % "zzz",
		......
	)
也可以用多行分开:

	libraryDependencies += "net.sourceforge.htmlcleaner" % "htmlcleaner" % "2.4"

	libraryDependencies += "net.sourceforge.htmlcleaner" % "htmlcleaner" % "2.4"

	libraryDependencies += "net.sourceforge.htmlcleaner" % "htmlcleaner" % "2.4"


