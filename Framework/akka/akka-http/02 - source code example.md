## source code example
### Actor
```scala
import java.util.concurrent.Executors

import akka.actor.{Actor, ActorLogging}
import com.typesafe.config.Config

import scala.concurrent.ExecutionContext
import com.windTa1ker.services.Constants._
import com.windTa1ker.services.marshallers.MyJsonProtocol

/**
  * Auther: jingqi
  * Date: 2019/8/12 11:52
  * Package: com.homework.services.flow_infrastructure.actors
  * Description: 
  *
  */
trait BaseActor extends Actor with ActorLogging with MyJsonProtocol{
    val config: Config
    implicit val ec = ExecutionContext.fromExecutor(Executors.newFixedThreadPool(config.getInt(THREAD_POOL)))
}

```
 