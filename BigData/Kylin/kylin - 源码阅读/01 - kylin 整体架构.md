#### Kylin 概览
::: hljs-center

![kylin 概览](../imgs/kylin_overview.PNG)

:::

#### Kylin 插件架构
![Kylin 插件架构](../imgs/kylin_plugin_architecture.png)

#### 如何运行
cube 元数据定义了 cube 所依赖的引擎，源和存储的类型。工厂模式用于构造每个依赖项的实例。适配器模式用于将部件连接在一起。
![How to run](../imgs/how_to_run.png)