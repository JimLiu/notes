这里面的Aggregate Resource Allocation : 38819 MB-seconds, 104 vcore-seconds是什么意思呢？

例如，我有一个单节点集群，其中，我已将每个容器的内存要求设置为：1228 MB（由config：yarn.scheduler.minimum-allocation-mb确定）和每个容器的vCore为1 vCore（由config确定： yarn.scheduler.minimum-allocation-vcores）。我已将：yarn.nodemanager.resource.memory-mb设置为9830 MB。因此，每个节点总共可以有8个容器（9830/1228 = 8）。因此，对于我的集群：VCores-Total = 1（节点）* 8（容器）* 1（每个容器的vCore）= 8

内存总数= 1（节点）* 8（容器）* 1228 MB（每个容器的内存）= 9824 MB = 9.59375 GB = 9.6 GB

下图显示了我的群集指标：现在让我们看看“MB-seconds”和“vcore-seconds”。根据代码中的描述（ApplicationResourceUsageReport.java）：MB-seconds：应用程序分配的聚合内存量（以兆字节为单位）乘以应用程序运行的秒数。vcore-seconds：应用程序分配的聚合数量的vcores乘以应用程序运行的秒数。描述是自我解释的（记住关键词：聚合）。让我用一个例子解释一下。我运行了一个DistCp作业（产生了25个容器），我得到了以下内容：聚合资源分配：10361661 MB-seconds，8424 vcore-seconds

现在，让我们粗略计算每个容器花费的时间：对于内存：

10361661 MB-seconds = 10361661/25（容器）/ 1228 MB（每个容器的内存）= 337.51秒= 5.62分钟

对于CPU

8424 vcore-seconds = 8424/25（容器）/ 1（每容器vCore）= 336.96秒= 5.616分钟

这表明平均每个容器需要5.62分钟才能执行。希望这说清楚。您可以执行某项工作并自行确认。