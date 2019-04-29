### 分层架构
1. 有状态的流式处理
	位于最底层，是 core Api 的底层实现。
	processingFunction
	灵活性高，但是开发比较复杂。
2. core APIs
	DataStream // 流式数据
	DataSet // 离线的计算
3. TABLE & SQL
	SQL 构建于 Table 之上，需要构建 Table 环境。
	Table 可以与 DataSet 与 DataStream 进行相互的转换。
	Streaming SQL 不同于存储的 sql, 最终会转化成流式执行计划。
### Flink 构建的