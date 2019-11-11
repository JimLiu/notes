## spray-json-test-extend

### 代码示例
```Scala
icit object CJsonFormat extends RootJsonFormat[C] {
        def write(c: C) = {
            c match {
                case t1: C1 => JsObject("name" -> JsString(t1.name))
                case t2: C2 => JsObject("age" -> JsNumber(t2.age))
                case t3: C3 => JsObject("name" -> JsString(t3.name), "age" -> JsNumber(t3.age))
            }
        }
        def read(value: JsValue) = { // 如果子类中的字段一样,目前我还不知道怎么处理
           new C()
        }
    }
    implicit object C1JsonFormat extends RootJsonFormat[C1] {
        def write(x: C1) = JsObject("name" -> JsString(x.name))
        def read(value: JsValue) = {
            value.asJsObject.getFields("name") match { // 类的继承, 集合的模式匹配
                case Seq(JsString(name)) => new C1(name)
                case _ =>  throw new DeserializationException("C1 expected")
            }
        }
    }
    implicit object C2JsonFormat extends RootJsonFormat[C2] {
        def write(x: C2) = JsObject("age" -> JsNumber(x.age))
        def read(value: JsValue) = {
            value.asJsObject.getFields("age") match {
                case Seq(JsNumber(age)) => new C2(age.toInt)
                case _ =>  throw new DeserializationException("C2 expected")
            }
        }
    }
    implicit object C3JsonFormat extends RootJsonFormat[C3] {
        def write(x: C3) = JsObject("name" -> JsString(x.name),
            "age" -> JsNumber(x.age))
        def read(value: JsValue) = {
            value.asJsObject.getFields("name","age") match {
                case Seq(JsString(name), JsNumber(age)) => new C3(name, age.toInt)
                case _ =>  throw new DeserializationException("C3 expected")
            }
        }
    }

    def main(args: Array[String]): Unit = {
        val obj: C= new C3("c3name", 10)
        println(obj.toJson.prettyPrint)
    }
}
```