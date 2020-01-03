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
1