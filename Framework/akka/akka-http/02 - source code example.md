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
### routes
```scala
import akka.actor.{ActorRef, ActorSystem}
import akka.event.LoggingAdapter
import akka.http.scaladsl.server.Route
import akka.util.Timeout
import com.windTa1ker.services.Constants
import com.typesafe.config.Config
import com.windTa1ker.services.marshallers.MyJsonProtocol
import spray.json.DefaultJsonProtocol

import scala.concurrent.duration._

trait BaseRoute extends MyJsonProtocol{
    import DefaultJsonProtocol._
    val config: Config
    val system: ActorSystem
    val log: LoggingAdapter
    val actor: ActorRef
    val userRoutes: Route
    val TMOUT: Int = config.getInt(Constants.ASK_TMOUT)
    implicit lazy val timeout: Timeout = Timeout(TMOUT.seconds)
    lazy val baseRoute: String = config.getString(Constants.BASE_ROUTE)
}

import akka.actor.{ActorRef, ActorSystem}
import akka.event.{Logging, LoggingAdapter}
import akka.util.Timeout
import com.windTa1ker.services.actors.IndexActor
import com.typesafe.config.Config
import akka.pattern.ask

import scala.concurrent.duration._
import akka.http.scaladsl.server.Directives._
import akka.http.scaladsl.server.Route
import IndexActor._
import com.windTa1ker.services.{ResultMessage, SqlResult}

class IndexRoute(val config: Config, val system: ActorSystem) extends BaseRoute {
    override val log: LoggingAdapter = Logging(system, classOf[IndexRoute])
    override val actor: ActorRef = system.actorOf(IndexActor.props(config), "indexActor")
    override implicit lazy val timeout = Timeout(20.seconds)
    lazy val userRoutes: Route = pathPrefix(baseRoute) {
        concat(
            pathEnd {
                get {
                    complete("hello world")
                }
            },
            pathPrefix("index") {
                get {
                    val hello = (actor ? Index).mapTo[ResultMessage[SqlResult]]
                    complete(hello)
                }
            },
            path(Segment) { name =>
                        concat(
                            get {
                                val info = (actor ? Index2(name)).mapTo[ResultMessage[SqlResult]]
                                complete(info)
                            }
                        )
            },
            pathPrefix("get") {
                        get {
                            parameters('cubeName).as(Index3){ gc =>
                                val info = (actor ? gc).mapTo[ResultMessage[SqlResult]]
                                complete(info)
                            }
                        }
            }
        )
    }
}
```
### Constants 
```scala
package com.windTa1ker.services


object Constants {

    /**
      * main config
      */
    val SERVICE_NAME = "main.service.name"
    val SERVICE_HOST = "main.service.host"
    val SERVICE_PORT = "main.service.port"
    val SERVICE_ROUTES = "main.service.routes"
    val BASE_ROUTE = "main.service.base.route"

    /**
      * route config
      */
    val ASK_TMOUT = "route.ask.timeout"

    /**
      * actor config
      */
    val THREAD_POOL = "actor.thread.pool"

    /**
      * envs config
      */
}
```
### Main
```scala
import java.io.File
import java.util

import com.typesafe.config.{Config, ConfigFactory}
import Constants._
import akka.actor.ActorSystem
import akka.http.scaladsl.Http
import akka.stream.ActorMaterializer
import com.windTa1ker.services.routes.BaseRoute
import scala.collection.JavaConversions._
import scala.concurrent.{Await, ExecutionContext, Future}
import scala.concurrent.duration.Duration
import scala.util.{Failure, Success, Try}
import akka.http.scaladsl.server.Directives._

object MainService {

    def main(args: Array[String]): Unit = {
        val config = if (args.length > 0 ) ConfigFactory.parseFile(new File(args(0))) else ConfigFactory.load()
        startServer(config)
    }

    def startServer(config: Config): Unit = {
        val serviceName = config.getString(SERVICE_NAME)
        val serviceHost = config.getString(SERVICE_HOST)
        val servicePort = config.getInt(SERVICE_PORT)
        implicit val system: ActorSystem = ActorSystem(serviceName, config)
        implicit val materializer: ActorMaterializer = ActorMaterializer()
        implicit val executionContext: ExecutionContext = system.dispatcher
        val routes = buildRoutes(config, system)
        val serverBinding: Future[Http.ServerBinding] = Http().bindAndHandle(routes, serviceHost, servicePort)
        serverBinding.onComplete {
            case Success(bound) =>
                println(s"Server online at http://${bound.localAddress.getHostString}:${bound.localAddress.getPort}/")
            case Failure(e) =>
                Console.err.println(s"Server could not start!")
                e.printStackTrace()
                system.terminate()
        }
        Await.result(system.whenTerminated, Duration.Inf)
    }

    def buildRoutes(config: Config, system: ActorSystem) = {
        val routeLst = config.getStringList(SERVICE_ROUTES)
        val routes= routeLst.flatMap(clazzName => {
            Try{
                println("classname: " + clazzName)
                val clazz = Class.forName(clazzName, true, Thread.currentThread().getContextClassLoader)
                val constructor =  clazz.getConstructor(classOf[Config], classOf[ActorSystem])
                constructor.newInstance(config, system).asInstanceOf[BaseRoute]
            } match {
                case Success(ok) => Some(ok)
                case Failure(err) =>
                    err.printStackTrace()
                    err.getMessage
                    None
            }
        })
        // println(routes)
        routes.map(_.userRoutes).reduceLeft(_ ~ _)
    }
}
```
### ResultMessage
```scala
case class ResultMessage[T](code: Int, msg: String, data: T) {

}

object ResultMessage {

    def success[T](data: T) = ResultMessage(0, "success", data)

    def fail[T](data: T) = ResultMessage(-1, "failed", data)

    def other[T](data: T) = ResultMessage(1, "other", data)

    def defaultSuccess(msg: String= "success"): ResultMessage[SqlResult] = success(SqlResult(List("info"), List(List(msg))))

    def defaultFail(msg: String="failed"): ResultMessage[SqlResult] = fail(SqlResult(List("info"), List(List(msg))))

    def defaultOther(msg: String = "other"): ResultMessage[SqlResult] = other(SqlResult(List("info"), List(List(msg))))
}

case class SqlResult(fields: List[String], results: List[List[String]])
```

### application.conf
```config

```





 