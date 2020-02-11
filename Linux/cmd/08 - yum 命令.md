# yum 命令
## 查询
    1. yum list # 查询所有可用的软件包列表
    2. yum search $关键字 # 搜索服务器上所有和关键字相关的包
## 安装
    1. yum -y install $包名
       选项: -y 自助回答 yes
       比如: yum -y install gcc
       
    2. 