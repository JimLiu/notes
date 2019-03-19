### Hybird 模型 (混合模型)
#### 参考文档
1. Problem
对于一个将要进行查询的 sql, kylin 会选择一个一个 cube 来响应对应的 query.
样例: 
&ensp;&ensp;&ensp;&ensp;假如我们有一个 cube 叫做 Cube_V1, 这个已经构建了两个月了。