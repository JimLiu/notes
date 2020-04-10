## 安装 ambari
### yum 源配置
#### 方法 1 公共 yum 源
    下载: http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.1.0/ambari.repo 放到 /etc/yum.repos.d .需要外网环境.
#### 方法 2 本地 yum 源
##### 1. 初始化环境
    1) 安装启动http、ntpd服务
    yum install httpd ntpd 
    service httpd start
    service ntpd start
    如果设置开机启动 chkconfig httpd on, chkconfig ntpd on
    2) 检查DNS和NSCD
    Ambari在安装时需要配置全域名，所以需要检查DNS。每台机器需要配置/etc/hosts以及/etc/sysconfig/network中的hostname 
    3) 关闭防火墙
    chkconfig iptables off 
    /etc/init.d/iptables stop
    关闭SELinux
    setenforce 0
    vi /etc/sysconfig/selinux 设置SELINUX=disable并重启机器
    4) 为每个机器创建ambari账号并设置密码
    useradd ambari && echo “ambari” | passwd --stdin ambari 
    5) 在server上安装本地源制作的相关工具 yum-utils createrepo
    yum install yum-utils createrepo
##### 2. 下载安装资源
    下载Ambar 2.4.1以及HDP 2.2.9.0和HDP-UTILS 1.1.0.20的安装资源，放到server机器上
        wget http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.1.0/ambari-2.4.1.0-centos6.tar.gz
        wget http://public-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.2.9.0/HDP-2.2.9.0-centos6-rpm.tar.gz
        wget http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.20/repos/centos6/HDP-UTILS-1.1.0.20-centos6.tar.gz
          
    在httpd的根目录下默认/var/www/html下创建ambari目录，并将下载的三个压缩包分别解压到目录下
    验证是否可用 访问 http://$ip/ambari  会显示出目录下的内容
        
    在 /var/www/html下执行 createrepo ambari 创建仓库信息文件

##### 3. 配置本地Ambari、HDP以及HDP-UTILS的源
    下载公共库的repo文件到/etc/yum.repos.d/目录下，然后修改其中的baseurl即可
        wget http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.1.0/ambari.repo
        wget http://public-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.2.9.0/hdp.repo
    将上面两个repo文件分别分发到安装机器的对应目录下。
    然后执行yum操作
        yum clean all
        yum list update
        yum makecache
        yum repolist
####  安装JDK、Python以及Mysql
     

### 问题
#### 在安装过程中如果oozie、hive等的由于jdbc的jar包原因未能安装成功，需要在hive的lib下拷贝关联jdbc的jar包。
    如果是hive执行下面命令，注意替换2.x中的版本
    ln -s /usr/share/java/mysql-connector-java.jar /usr/hdp/2.x/hive/lib/mysql-connector-java.jar
    ln -s /usr/share/java/mysql-connector-java.jar /usr/hdp/2.x/hive2/lib/mysql-connector-java.jar
    如果是oozie的话，需要执行
    ambari-server setup --jdbc-db=mysql --jdbc-driver=/usr/share/java/mysql-connector-java.jar
    然后在amber的页面上重启。
#### 
    如果配置ambari hive view时候出现mysql报错，注意替换/var/lib/ambari-server/resource 目录下相应版本的mysql的jdbc的mysql-connector-java.jar包。
#### 
    hiveserver2的authentication的选项注意选择NOSASL
#### 
    如果在节点上自己安装了mysql高版本，在ambari部署时会出现mysql安装错误，此时注意更改相应节点的部署脚本的依赖检查部分，让mysql检查通过。
    操作更改：/usr/lib/python2.6/site-packages/resource_management/core/providers/package/yumrpm.py的_check_existence方法，如果依赖
    检查name为mysql直接返回true
#### centos7 ambari页面上看agent已经lost heartbeat
    修改/etc/ambari-agent/conf/ambari-agent.ini 在[security]下添加force_https_protocol=PROTOCOL_TLSv1_2
    修改/etc/python/cert-verification.cfg设置verify=disable
    重启下agent：service ambari-agent restart