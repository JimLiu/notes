
### 概念
* 项目: zyb-code: scala.akka
* 消息必需是不可变的，因为他们会在不同的线程中共享。
* 消息最好写在 actor 的伴生对象中。
* actor 内部的成员变量都是他的状态，并且 actor 模型保证内部的状态是线程安全的。
* 位置透明:
* props method 决定如何创建这个 actor:
```scala
object Greeter {
  def props(message: String, printerActor: ActorRef): Props = Props(new Greeter(message, printerActor))
  final case class WhoToGreet(who: String)
  case object Greet
}
```
### 位置透明:
在akka 中， 你不能通过 new 这个关键字来创建 akka, 相反的需要通过 工厂来创建，并且返回的是一个 actorRef 而不是一个真正的 actor, 这样带来了很多的便捷。
xxx
* actorSystem 作为 actor 的工厂，也管理着 actor 的生命周期。actor 和 actorSystem 的 name 需要是唯一的。
**正确**
```scala
// Create the printer actor
val printer: ActorRef = system.actorOf(Printer.props, "printerActor")

// Create the 'greeter' actors
val howdyGreeter: ActorRef =
  system.actorOf(Greeter.props("Howdy", printer), "howdyGreeter")
val helloGreeter: ActorRef =
  system.actorOf(Greeter.props("Hello", printer), "helloGreeter")
val goodDayGreeter: ActorRef =
  system.actorOf(Greeter.props("Good day", printer), "goodDayGreeter")
```
**错误**
```scala
// Create the 'helloAkka' actor system
        val system: ActorSystem = ActorSystem("helloAkka")

        //#create-actors
        // Create the printer actor
        val printer: ActorRef = system.actorOf(Printer.props, "printerActor")

        // Create the 'greeter' actors
        val howdyGreeter: ActorRef =
            system.actorOf(Greeter.props("Howdy", printer), "howdyGreeter") //与下面的相同程序会报错
        val helloGreeter: ActorRef =
            system.actorOf(Greeter.props("Hello", printer), "howdyGreeter") //错误
        val goodDayGreeter: ActorRef =
            system.actorOf(Greeter.props("Good day", printer), "goodDayGreeter")
```
### 异步通信
actor 是响应式的，也是事件驱动的。actors 之间的通信是异步的，也就是说，发送者发送消息后，不用等待接收者回复消息。接收者的邮箱是一个有序的消息队列。来自同一个 actor 的消息一定是有序的，并且，也有可能中间插入别的 actor 的消息。

### 测试 actor
1. https://developer.lightbend.com/guides/akka-quickstart-scala/testing-actors.html
2. https://doc.akka.io/docs/akka/current/testing.html?language=scala

### 

