### SBT
#### repo:
[repositories]
  local
  maven-central
  maven-local: file://E:/envs/apache-maven-3.5.3-bin/repo
  aliyun-nexus: http://maven.aliyun.com/nexus/content/groups/public/  
  typesafe: http://repo.typesafe.com/typesafe/ivy-releases/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext], bootOnly
  ivy-sbt-plugin:http://dl.bintray.com/sbt/sbt-plugin-releases/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext]
  central: http://repo1.maven.org/maven2/
  ibiblio-maven: http://maven.ibiblio.org/maven2/
  uk-repository: http://uk.maven.org/maven2/
  jboss-repository: http://repository.jboss.org/nexus/content/groups/public/
  conjars: http://conjars.org/repo
  Pentaho: http://repo.pentaho.org/artifactory/repo

#### sbtconfig.txt
\# Set the java args to high

-Xmx1024M

-XX:MaxPermSize=512m

-XX:ReservedCodeCacheSize=256m



\# Set the extra SBT options

-Dsbt.log.format=true

\# self configuration:  ## ========================== ##
-Dsbt.repository.config=E:/envs/sbt-1.2.7/sbt/conf/repo.properties
-Dsbt.override.build.repos=true
\#-Dhttp.proxyHost=localhost
\#-Dhttp.proxyPort=8080
\#-Dsbt.gigahorse=false
\#-Dsbt.repository.secure=false
\## ========================== ##