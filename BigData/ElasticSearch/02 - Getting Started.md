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
       或者启动时指定集群以及节点名称：./elasticsearch -Ecluster.name=my_cluster_name -Enode.name=my_node_name

### 查看集群
    通过 es 提供的 api:
    (1) 检查 集群，节点，index 健康情况，状态，以及 统计数据
    (2) 管理 集群，节点，索引，元数据
    (3) crud 操作，查询对应的index
    (4) 执行 一些高级操作：paging, sorting, filtering, scripting, aggregations, and etc...
    
    1. 集群健康
    (1) 查看集群情况：curl -X GET "localhost:9200/_cat/health?v&pretty"
    (2) 查看集群节点：curl -X GET "localhost:9200/_cat/nodes?v&pretty"
   
    2. 查看所有索引
    (1) curl -X GET "localhost:9200/_cat/indices?v&pretty"
    
    3. 创建索引
    (1) curl -X PUT "localhost:9200/customer?pretty&pretty"
    查看索引：curl -X GET "localhost:9200/_cat/indices?v&pretty"

    4. 索引和查询 Document
    (1) curl -X PUT "localhost:9200/customer/_doc/1?pretty&pretty" -H 'Content-Type: application/json' -d'
    {
      "name": "John Doe"
    }'

    (2) curl -X GET "localhost:9200/customer/_doc/1?pretty&pretty"
    
    5. 删除索引
    (1) curl -X DELETE "localhost:9200/customer?pretty&pretty"
    (2) curl -X GET "localhost:9200/_cat/indices?v&pretty"
    6. 补充：<REST Verb> /<Index>/<Type>/<ID>
   
### 修改数据
    1. 更新文档
    curl -X POST "localhost:9200/customer/_doc/1/_update?pretty&pretty" -H 'Content-Type: application/json' -d'
{
  "doc": { "name": "Jane Doe" }
}
'

 


      