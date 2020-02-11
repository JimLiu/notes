# yum 命令
## 查询
    1. yum list # 查询所有可用的软件包列表
    2. yum search $关键字 # 搜索服务器上所有和关键字相关的包
## 安装
    1. yum -y install $包名
       选项: -y 自助回答 yes
       比如: yum -y install gcc
       yum 只需要提供包名就行. 不用提供包全名
## 升级
    1. yum -y update $包名
       -- 选项同上 
       千万不要直接执行: yum -y update # 后面不带包名, 因为会升级所有的软件, 包括系统内核
## 卸载
    1. yum -y remove $包名 # 不建议使用这个命令卸载相应软件

## yum 软件组管理命令
    1. yum grouplist # 列出所有可用的软件组列表
    2. yum groupinstall $软件组名 # 安装指定软件组 
    3. yum groupremove 软件组名 # 卸载指定软件组