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

import akka.actor.Props
import com.windTa1ker.services.{ResultMessage, SqlResult}
import com.typesafe.config.Config
import spray.json._

class IndexActor(val config: Config) extends BaseActor {
    import IndexActor._
    override def receive: Receive = {
        case Index =>
            log.info("Index: " + Index)
            val sqlResult = SqlResult(List("hello", "world"), List(List("hello", "world")))
            val resultMessage = ResultMessage(0, "hello world", sqlResult)
            sender() ! resultMessage
    }
}

object IndexActor {
    def props(config: Config) = Props(new IndexActor(config))
    // protocols
    case object Index
}
```
### marshallers
```scala
import akka.http.scaladsl.marshallers.sprayjson.SprayJsonSupport
import com.windTa1ker.services.{ResultMessage, SqlResult}

trait MyJsonProtocol extends SprayJsonSupport{
    import spray.json._
    import DefaultJsonProtocol._
    implicit val sqlResultFmt: RootJsonFormat[SqlResult] = jsonFormat2(SqlResult)
    implicit def resultMessageFmt[A :JsonFormat]: RootJsonFormat[ResultMessage[A]] = jsonFormat3(ResultMessage.apply[A])
}
```
### models
### rou
 