### Kylin Source
#### 三个设计模式
1. 工厂模式
生成数据源，生成计算引擎，生成数据存储。
--，EngineFactory, StorageFactory
2. 适配器模式
两种实现：
(1): 通过继承的方式实现适配器模式。(也是 Kylin 采用的方法)
![extends](../imgs/adapter_extend.png)
(2): 通过委托的方式实现适配器模式
PrintBanner 实现 Print 抽象类，
![extends](../imgs/adapter_delegation.png)
3. Builder 模式

