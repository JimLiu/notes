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
       或者启动时指定集群以及节点名称：$ES_HOME/bin/elasticsearch -Ecluster.name=cluster_xxx -Enode.name=node_xxx

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
    (1) curl -X POST "localhost:9200/customer/_doc/1/_update?pretty&pretty" -H 'Content-Type: application/json' -d'
    {
      "doc": { "name": "Jane Doe" }
    }'
    (2) curl -X POST "localhost:9200/customer/_doc/1/_update?pretty&pretty" -H 'Content-Type: application/json' -d'
    {
      "doc": { "name": "Jane Doe", "age": 20 }
    }'
    (3) curl -X POST "localhost:9200/customer/_doc/1/_update?pretty&pretty" -H 'Content-Type: application/json' -d'
    {
      "script" : "ctx._source.age += 5"
    }'

    2. 删除文档
    curl -X DELETE "localhost:9200/customer/_doc/2?pretty&pretty"

    3. 批处理
    (1) curl -X POST "localhost:9200/customer/_doc/_bulk?pretty&pretty" -H 'Content-Type: application/json' -d'
    {"index":{"_id":"1"}}
    {"name": "John Doe" }
    {"index":{"_id":"2"}}
    {"name": "Jane Doe" }'
    (2) curl -X POST "localhost:9200/customer/_doc/_bulk?pretty&pretty" -H 'Content-Type: application/json' -d'
    {"update":{"_id":"1"}}
    {"doc": { "name": "John Doe becomes Jane Doe" } }
    {"delete":{"_id":"2"}}'

### Exploring Data
    数据集地址：https://github.com/elastic/elasticsearch/blob/master/docs/src/test/resources/accounts.json?raw=true
    导入数据：curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_doc/_bulk?pretty&refresh" --data-binary "@accounts.json"
    查看索引：curl "localhost:9200/_cat/indices?v"
    
    1. Search Api
    curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match_all": {} },
      "sort": [
        { "account_number": "asc" }
      ]
    }'

    2. Introducing the Query Language 
    (1) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
       "query": { "match_all": {} }
    }'

    (2) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
       "query": { "match_all": {} },
       "size": 1
    }'

    (3) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match_all": {} },
      "from": 10,
      "size": 10
    }'

    (4) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match_all": {} },
      "sort": { "balance": { "order": "desc" } }
    }'
 
    3. Executing Searches
    (1) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match_all": {} },
      "_source": ["account_number", "balance"]
    }'

    (2) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match": { "account_number": 20 } }
    }'
    (3) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match": { "address": "mill" } }
    }'
    (4) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match": { "address": "mill lane" } }
    }'
    (5) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
      "query": { "match_phrase": { "address": "mill lane" } }
    }'
    (6) curl -X GET "localhost:9200/bank/_search?pretty" -H 'Content-Type: application/json' -d'
    {
    "query": {
        "bool": {
        "must": [
            { "match": { "address": "mill" } },
            { "match": { "address": "lane" } }
        ]
        }
    }
    }'
    (7) 






    
 


      