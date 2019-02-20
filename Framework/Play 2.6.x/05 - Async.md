1. 返回 Future[Result]
如果需要返回 Future[Result], 那么，其实客户端可能还是需要阻塞等待服务器返回对应的结果。
如果需要马上给客户端返回一个结果，同时服务端在另外一个线程里面开始执行其它的逻辑。可以参考以下代码：
```scala
def fun() = Action {
	Future{
		xxxxxx
	}(myContext)
	Ok("result")
}
```
2. 建议不要使用默认的环境执行异步操作，可以新建自己的线程池，在新的线程池中执行异步的代码
```scala
import play.api.libs.concurrent.CustomExecutionContext

// Make sure to bind the new context class to this trait using one of the custom
// binding techniques listed on the "Scala Dependency Injection" documentation page
trait MyExecutionContext extends ExecutionContext

class MyExecutionContextImpl @Inject()(system: ActorSystem)
  extends CustomExecutionContext(system, "my.executor") with MyExecutionContext

class HomeController @Inject()(myExecutionContext: MyExecutionContext, val cc: ControllerComponents) extends AbstractController(cc) {
  def index = Action.async {
    Future {
      // Call some blocking API
      Ok("result of blocking call")
    }(myExecutionContext)
  }
}
```
3. Action 本身就是异步的
