# 生成 k 进制串

``` scala
    val buffer = ArrayBuffer(0, 0, 0, 0, 0)

    def kString(n: Int, k: Int): Unit = { // n, buffer 的长度, k: k 进制
        n < 1 match {
            case true =>
                println(buffer.reverse.mkString(""))
            case false =>
                (0 until k).foreach(x => {
                    buffer(n - 1) = x
                    kString(n - 1, k)
                })
        }
    }
``` 