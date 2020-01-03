## 安装 MYSQL
1. 下载 mysql tar 包, 并解压
2. mv mysql-xxx /usr/local/
3. sudo ln -s ${MYSQL_HOME} /usr/local/mysql
4. cd /usr/local/mysql
5. bin/mysqld --initialize --user=windta1ker --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
6. support-files/mysql.server start
7. bin/mysql -uroot -p
8. ALTER USER 'root'@'localhost' IDENTIFIED BY 'xxx';
9. 配置文件
``` text
[mysqld]
#设置3306端口
port = 3306
#设置mysql客户端默认字符集
character-set-server=utf8
# 设置mysql的安装目录
basedir=/usr/local/mysql
# 设置mysql数据库的数据的存放目录
datadir=/usr/local/mysql/data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```