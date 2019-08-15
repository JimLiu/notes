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
main.service {
  name = "my-service"
  port = 8920
  host = localhost
  routes = ["com.windTa1ker.services.routes.IndexRoute",
  "com.windTa1ker.services.routes.CubeRoute",
  "com.windTa1ker.services.routes.DataModelRoute"]
  base.route = "route-base"
}

# config akka
akka{
  loggers = ["akka.event.slf4j.Slf4jLogger"]
  loglevel = "DEBUG"
  logging-filter = "akka.event.slf4j.Slf4jLoggingFilter"
}

# route config
route.ask.timeout = 5

# actor config
actor.thread.pool = 20

# envs config
```

### pom
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.windTa1ker.services</groupId>
    <artifactId>my-service</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <scala.version>2.11.12</scala.version>
        <scala.big.version>2.11</scala.big.version>
        <scala.binary.version>2.11</scala.binary.version>
        <scala.test.verion>3.0.5</scala.test.verion>
        <akka.version>2.5.19</akka.version>
        <akka.http.version>10.1.8</akka.http.version>
        <logback.version>1.2.3</logback.version>
        <hadoop.version>2.7.3</hadoop.version>
    </properties>

    <repositories>
        <repository>
            <id>1</id>
            <name>pentaho</name>
            <url>http://repo.pentaho.org/artifactory/repo</url>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>com.typesafe</groupId>
            <artifactId>config</artifactId>
            <version>1.3.4</version>
        </dependency>
        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-actor_${scala.big.version}</artifactId>
            <version>${akka.version}</version>
        </dependency>
        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-http_${scala.big.version}</artifactId>
            <version>${akka.http.version}</version>
        </dependency>
        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-http-spray-json_${scala.big.version}</artifactId>
            <version>${akka.http.version}</version>
        </dependency>
        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-http-xml_${scala.big.version}</artifactId>
            <version>${akka.http.version}</version>
        </dependency>
        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-stream_${scala.big.version}</artifactId>
            <version>${akka.version}</version>
        </dependency>
        <dependency>
            <groupId>org.scalaj</groupId>
            <artifactId>scalaj-http_${scala.big.version}</artifactId>
            <version>2.3.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.7</version>
        </dependency>
        <dependency>
            <groupId>com.typesafe.akka</groupId>
            <artifactId>akka-slf4j_${scala.big.version}</artifactId>
            <version>${akka.version}</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>${logback.version}</version>
            <exclusions>
                <exclusion>
                    <artifactId>org.slf4j</artifactId>
                    <groupId>slf4j-api</groupId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
    <build>
        <sourceDirectory>src/main/scala</sourceDirectory>
        <testSourceDirectory>src/test/scala</testSourceDirectory>
        <plugins>
            <plugin>
                <groupId>net.alchim31.maven</groupId>
                <artifactId>scala-maven-plugin</artifactId>
                <version>3.2.2</version>
                <executions>
                    <execution>
                        <id>compile-scala</id>
                        <phase>compile</phase>
                        <configuration>
                            <source>1.8</source>
                            <target>1.8</target>
                        </configuration>
                        <goals>
                            <goal>add-source</goal>
                            <goal>compile</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>test-compile-scala</id>
                        <phase>test-compile</phase>
                        <goals>
                            <goal>add-source</goal>
                            <goal>testCompile</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <skip>true</skip>
                    <scalaVersion>${scala.version}</scalaVersion>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.18</version>
                <configuration>
                    <skip>true</skip>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>3.0.0</version>
                <executions>
                    <execution>
                        <id>distro-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <appendAssemblyId>false</appendAssemblyId>
                    <descriptors>
                        <descriptor>assembly.xml</descriptor>
                    </descriptors>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### logback
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="FILE"
              class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>

        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logs/akka.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>50MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
    </appender>

    <root level="DEBUG">
        <appender-ref ref="FILE" />
    </root>
</configuration>
```






 