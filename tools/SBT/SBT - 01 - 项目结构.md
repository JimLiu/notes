### 项目结构
#### 基于 Shell
```BASH
#!/bin/bash
mkdir -p src/{main,test}/{java,resources,scala}
mkdir lib project target
# create an initial build.sbt file
echo 'name := "MyProject"

version := "1.0"

scalaVersion := "2.11.12"' > build.sbt
```
#### 基于 Giter8
GitHub: https://github.com/n8han/giter8/wiki/giter8-templates
eg: sbt new scala/hello-world.g8
