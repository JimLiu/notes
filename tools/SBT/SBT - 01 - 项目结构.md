### 项目结构
#### 基于 Shell
```BASH
#!/bin/bash
mkdir -p src/{main, test}/{java, resources, scala}
mkdir lib project target
# create an initial build.sbt file
echo 'name := "MyProject"

version := "1.0"

scalaVersion := "2.11.12"' > build.sbt
```
#### 