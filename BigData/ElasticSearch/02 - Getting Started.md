## Getting Started
    使用案例：
    - 商品查询
    - 交易日志搜集，灌入 es, 并进行分析
    - 降价提醒
    - 近实时分析
### Basic Concepts
    1. NRT Near Realtime 
    2. Cluster 
    3. Node
    4. Index
       Index 是属性的集合，
    5. Type (Deprecated in 6.0.0) 
       相当于表名。6.0.0 已经标记成过时了，在 6.x 中还可以使用。8.x 版本彻底不能用。
    6. Document
    7. Shards & Replicas
### Installation
    1. Elasticsearch 要求 Java 8. 推荐使用： Oracle JDK version 1.8.0_131。
       下载地址：https://www.elastic.co/cn/downloads/past-releases#elasticsearch 
    2. 启动：cd elasticsearch-6.3.2/bin
    3. 