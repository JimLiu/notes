### 分层架构
1. 有状态的流式处理
	位于最底层，是 core Api 的底层实现。
	processingFunction
	灵活性高，但是开发比较复杂。
2. core APIs
	DataStream // 流式数据
	DataSet // 离线的计算
3. TABLE & SQL
	