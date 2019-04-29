## Flink 运行时架构
### Client

### JobManager 
### TaskManager
TaskManager 是一个进程，Slots 由各个线程执行
1. CoLocationGroup
	保证所有 i-th 的 sub-tasks 在同一个 slots
	主要用于迭代流
2. SlotSharingGroup
	1. 保证同一个 group 的 i-th 的sub-tasks 共享同一个 slots
	2. 片子默认的 group 为 default
	3. 怎么确定一个算子的 SlotSharingGroup(根据input的group和自身是否设置group共同确定)
	4. 适当设置可以减少每个 slot 运行的线程数，从而整体上减少机器的负载
3. slots && parallelism
4. Operr
### 角色间通信（akka）
### 数据的传输（Netty）

