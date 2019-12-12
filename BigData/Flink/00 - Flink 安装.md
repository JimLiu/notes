## Flink 安装
### 自己编译  
    mvn clean package -DskipTest
### 问题整理
    1. 启动 wordcount: ~/envs/flink-1.9.1/bin/flink run -m yarn-cluster -yqu $queuename ~/envs/flink-1.9.1/examples/batch/WordCount.jar  
       报错: Could not identify hostname and port in 'yarn-cluster'
       原因: Flink1.8中，FIX了FLINK-11266,将flink的包中对hadoop版本进行了剔除，导致flink中直接缺少hadoop的client相关类，无法解析yarn-cluster参数.  
       解决: export HADOOP_CLASSPATH=`hadoop classpath`
2.