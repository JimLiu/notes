## Flink 运行时架构
### Client

### JobManager 
### TaskManager
TaskManager 是一个进程，Slots 由各个线程执行
1. CoLocationGroup
	保证所有 i-th 的 sub-tasks 在同一个 slots
	主要用于迭代流
2. SlotSharingGroup
	1. 
### 角色间通信（akka）
### 数据的传输（Netty）

