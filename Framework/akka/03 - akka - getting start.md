### 现在系统需要一个新的编程模型
### actor 模型如何满足当前需要
#### actor 如何优雅的处理错误情况

1. 任务本身出错
actor 返回调用者错误信息
1. actor 出错
actor 被处理成具有树形结构模型。
parent actor 出错，child actor 递归的被关闭
child actor 出错，parent actor 知道如何处理
::: hljs-center
![image.png](0)
:::

