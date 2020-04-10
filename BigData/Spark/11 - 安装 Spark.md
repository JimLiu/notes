## 安装 Spark
### 下载
    https://mirrors.huaweicloud.com/apache/spark/
### 配置环境变量
    SPARK_HOME
    HADOOP_HOME
    HADOOP_CONF_DIR

    export PATH=$SPARK_HOME:/bin:$PATH
### 修改 spark-defaults.conf, 增加 hdp 配置
    spark.driver.extraJavaOptions      -Dhdp.version=2.6.5.1175
    spark.executor.extraJavaOptions    -Dhdp.version=2.6.5.1175
    spark.yarn.am.extraJavaOptions     -Dhdp.version=2.6.5.1175
### 补充 jersey 包
    jersey-client-1.9.jar
    jersey-core-1.9.jar
    注: 不要添加 jersey-server-1.9.jar 这个 jar 包, 会与 spark ui 冲突导致 spark ui 上无法查看 all exe
### spark-shell 测试
    spark-shell --master yarn