## 安装 ambari



### 4. 问题
#### 4.1 在安装过程中如果oozie、hive等的由于jdbc的jar包原因未能安装成功，需要在hive的lib下拷贝关联jdbc的jar包。
    如果是hive执行下面命令，注意替换2.x中的版本
    ln -s /usr/share/java/mysql-connector-java.jar /usr/hdp/2.x/hive/lib/mysql-connector-java.jar
    ln -s /usr/share/java/mysql-connector-java.jar /usr/hdp/2.x/hive2/lib/mysql-connector-java.jar
    如果是oozie的话，需要执行
    ambari-server setup --jdbc-db=mysql --jdbc-driver=/usr/share/java/mysql-connector-java.jar
    然后在amber的页面上重启。
#### 4.2 
    如果配置ambari hive view时候出现mysql报错，注意替换/var/lib/ambari-server/resource 目录下相应版本的mysql的jdbc的mysql-connector-java.jar包。
#### 4.3 
    hiveserver2的authentication的选项注意选择NOSASL
#### 4.4 
    如果在节点上自己安装了mysql高版本，在ambari部署时会出现mysql安装错误，此时注意更改相应节点的部署脚本的依赖检查部分，让mysql检查通过。
    操作更改：/usr/lib/python2.6/site-packages/resource_management/core/providers/package/yumrpm.py的_check_existence方法，如果依赖
    检查name为mysql直接返回true
#### 4.5 centos7 ambari页面上看agent已经lost heartbeat
    修改/etc/ambari-agent/conf/ambari-agent.ini 在[security]下添加force_https_protocol=PROTOCOL_TLSv1_2
    修改/etc/python/cert-verification.cfg设置verify=disable
    重启下agent：service ambari-agent restart