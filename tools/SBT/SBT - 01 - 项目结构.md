### 项目结构
#### 基于 Shell
#!/bin/bash
mkdir -p src/{main, test}/{java, resources, scala}
mkdir lib project target
# create an initial build.sbt file
echo 'name := "MyProject"

version := ""
'