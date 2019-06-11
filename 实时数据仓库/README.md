### 实时数据仓库
实时数据源：
    kafka: topic -> hive table
    消息队列
    
实时 ETL:
    数据源实时标准化：schema
    
实时计算并存储: 实时：5分钟，10分钟
    spark streaming, flink, druid, kylin
    计算类型：
        有状态的计算:
            登录注册，uv
        局部有状态的计算:
        无状态的计算:
    存储:
        hbase/redis    
实时查询:
    hbase  
故障的即时恢复: