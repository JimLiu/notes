### github 依赖
project/Build.scala
import sbt._
object MyBuild extends Build {
	lazy val root = Project("root", file(".")) dependOn(soudPlayerProject) dependOn(appleScriptUtils)
	lazy val soundPlayerProject = RootProject(uri("git://github.com/alvinj/SoundFilePlayer.git"))
	lazy val soundPlayerProject = RootProject(uri("git://github.com/alvinj/appleScriptUtils.git"))
}