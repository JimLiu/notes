### 指定 Main
在 build.sbt 中加入：

	mainClass in (Compile, run) := Some("com.alvinalexander.foo")

当应用被打包成 jar 文件时，要指定添加到清单的类，在 build.sbt 中加入下面这行：

	mainClass in (Compile, packageBin) := Some("com.alvinalexander")