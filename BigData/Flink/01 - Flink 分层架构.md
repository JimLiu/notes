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

### Flink 构建的流程
1. 构建计算环境
2. 创建 Source
3. 对于数据进行不同的转换 // map ......
4. 对于结果进行Sink

### window
1. 什么是 window
2. Window 类型：
	Count Window // 事件的条数
	Time Window // 时间的窗口
	自定义 Window
3. Window 聚合日常会遇到的问题:
	数据过热，延迟数据丢弃，反压等问题

### Time 类型
1. Event Time // 事件本身的时间
2. Ingestion Time // 进行Flink 的时间
3. Processing Time // window 处理的时间

### State
1. 什么是状态，状态托管
2. Operator State
3. Keyed State
4. State Backend (rocksdb + hdfs)

### checkpoint
checkpoint state 通常与checkpoint 机制结合使用
1. 轻量级的容错机制
2. 保证 exactly-once 语义（保证flink 内部失败，不保证外部的 exctly-once）
3. 用于内部失败的恢复
4. 基本原理:
	* 通过往 source 注入 barrier
	* barrier 作为 checkpoint 的标志

### savepoint
1. 流式处理过程中的状态历史版本
2. 具有可以replay的功能
3. 外部恢复（应用重启和升级）
4. 两种方式触发
	* Cancel with savepoint
	* 手动主动触发




