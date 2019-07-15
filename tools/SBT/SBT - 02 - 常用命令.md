### 常用命令
#### 编译，运行，打包
1. sbt compile
2. sbt run
3. sbt package
#### 交互模式
> $ sbt
> \> compile
> \> run
> \> package
#### 可以一次运行多个命令：
> clean compile 

```
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
&```