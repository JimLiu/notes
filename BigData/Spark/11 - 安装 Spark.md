## 安装 Spark
### 下载
    
### 修改 spark-defaults.conf
    spark.driver.extraJavaOptions      -Dhdp.version=2.6.5.1175
    spark.executor.extraJavaOptions    -Dhdp.version=2.6.5.1175
    spark.yarn.am.extraJavaOptions     -Dhdp.version=2.6.5.1175