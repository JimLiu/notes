### Hybird 模型 (混合模型)
#### 参考文档 &ensp;&ensp; [Kylin Hybird](http://kylin.apache.org/blog/2015/09/25/hybrid-model/)
1. Problem
对于一个将要进行查询的 sql, kylin 会选择一个一个 cube 来响应对应的 query.
样例: 
&ensp;&ensp;&ensp;&ensp;假如我们有一个 cube 叫做 Cube_V1, 这个已经构建了两个月了。现在，用户想要增加新的维度以及度量来满足他们 当前的需求。所以他们又建了 Cube_V2。
由于一些原因，用户想要保留 Cube_V1, 同时希望 Cube_V2 在Cube_V1 最后的账期开始进行计算。    

    * 历史数据已经被删除了，Cube_V2 不能从最开始的时候进行构建。
    * cube 很大，从头构建需要花大量的时间。
    * 新的维度以及度量只是从某天开始有，之前的历史数据中是没有相应的字段信息的。 
    * 用户觉得历史数据中，没有相应的字段也是可以接受的。
#### Hybird Model
::: hljs-center

![Hybird](../../imgs/Hybird.png)

:::
Hybird 模型是由多个其它的模型混合而成的。
他相当于其它