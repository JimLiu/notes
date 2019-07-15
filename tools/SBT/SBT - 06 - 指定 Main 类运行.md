### 指定 Main
在 build.sbt 中加入：

	mainClass in (Compile, run) := Some("com.alvinalexander.foo")

当应用被打包成 jar 文件时，