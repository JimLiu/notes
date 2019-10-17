# AKKA - HTTP
## 学习资料
1. spray-json:    
    https://github.com/spray/spray-json
2. spray-json with enum:
    https://stackoverflow.com/questions/46612186/scala-spray-json-universal-enumeration-json-formatting
3. case class with companion object
    https://index.scala-lang.org/spray/spray-json/spray-json/1.2.5
    在上面的页面中，搜索：Color.apply
4. 处理 null
    https://stackoverflow.com/questions/52854700/spray-deserialization-issue 
```scala
def main(args: Array[String]): Unit = {
        val str = """[{"f1":"v1","f2":"v2"},{"f3":"v3","f4":"v4"},null,null]"""
        val jsv: Seq[Option[JsValue]] = str.parseJson.convertTo[List[Option[JsValue]]]
        println(jsv.toString())
    }
```

5. spray-json 源码分析
    http://fangjian0423.github.io/2015/12/23/scala-spray-json/
5. akka http 入门项目
    https://developer.lightbend.com/guides/akka-http-quickstart-scala/
6. 