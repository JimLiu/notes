
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
* 位置透明:
在akka 中， 你不能通过 new 这个关键字来创建 akka, 相反的需要通过 工厂来创建，并且返回的是一个 actorRef 而不是一个真正的 actor, 
