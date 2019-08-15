## source code example
### Actor
```scala
import java.util.concurrent.Executors

import akka.actor.{Actor, ActorLogging}
import com.typesafe.config.Config

import scala.concurrent.ExecutionContext
import com.windTa1ker.services.Constants._
import com.windTa1ker.services.marshallers.MyJsonProtocol

trait BaseActor extends Actor with ActorLogging with MyJsonProtocol{
    val config: Config
    implicit val ec = ExecutionContext.fromExecutor(Executors.newFixedThreadPool(config.getInt(THREAD_POOL)))
}


```
 