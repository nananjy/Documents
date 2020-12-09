# Mysql

## 安装Mysql
1. 下载mysql-5.7.32-linux-glibc2.12-x86_64.tar.gz
2. 新建目录mkdir /usr/local/mysql/data
3. 新建用户及用户组
>[root@izuf6anc2b2vgf95tkc5y4z mysql]# groupadd mysql
><br/>[root@izuf6anc2b2vgf95tkc5y4z mysql]# useradd -g mysql mysql
4. 修改mysql目录用户组及权限
>chown -R mysql:mysql /usr/local/mysql
><br/>chmod -R 755 /usr/local/mysql
5. 编译安装并初始化mysql,务必初始化输出日志末尾的密码（数据库管理员临时密码）
>cd /usr/local/mysql/bin
><br/>./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql
<blockquote>
<b><i>补充说明：</i></b>
<br/>第5步 可能会出现错误
<br/>./mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
<br/>该问题出现首先检查有没有指定文件，没有则安装
<br/>rpm -qa | grep libaio
<br/>yum install libaio-devel.x86_64
</blockquote>

6. 运行初始化命令成功，日志
```
[root@izuf6anc2b2vgf95tkc5y4z bin]# ./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql
2020-11-21T05:26:53.813451Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2020-11-21T05:26:54.824500Z 0 [Warning] InnoDB: New log files created, LSN=45790
2020-11-21T05:26:54.942788Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2020-11-21T05:26:55.010515Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 2afd93f0-2bba-11eb-9338-00163e2393cf.
2020-11-21T05:26:55.012484Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2020-11-21T05:26:55.738135Z 0 [Warning] CA certificate ca.pem is self signed.
2020-11-21T05:26:55.800368Z 1 [Note] A temporary password is generated for root@localhost: dtZeMhf0J%jS
```
7. 编辑配置文件my.cnf，添加配置如下
```
[root@izuf6anc2b2vgf95tkc5y4z bin]# vi /etc/my.cnf

[mysqld]
datadir=/usr/local/mysql/data
port=3306
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
symbolic-links=0
max_connections=600
innodb_file_per_table=1
lower_case_table_names=1
character_set_server=utf8
```
`lower_case_table_names`：是否区分大小写，1表示存储时表名为小写，操作时不区分大小写；0表示区分大小写；不能动态设置，修改后，必须重启才能生效：
<br/>`character_set_server`：设置数据库默认字符集，如果不设置默认为latin1
<br/>`innodb_file_per_table`：是否将每个表的数据单独存储，1表示单独存储；0表示关闭独立表空间，可以通过查看数据目录，查看文件结构的区别

8. 测试启动Mysql
>[root@izuf6anc2b2vgf95tkc5y4z log-pid]# /usr/local/mysql/support-files/mysql.server start
><br/>Starting MySQL.                                            [  OK  ]
9. 添加软连接，并重启mysql服务
```
[root@izuf6anc2b2vgf95tkc5y4z log-pid]# ln -s /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
[root@izuf6anc2b2vgf95tkc5y4z log-pid]# ln -s /usr/local/mysql/bin/mysql /usr/bin/mysql
[root@izuf6anc2b2vgf95tkc5y4z log-pid]# service mysql restart
Shutting down MySQL..                                      [  OK  ]
Starting MySQL.                                            [  OK  ]
```
10. 登录mysql，修改密码(密码为步骤5生成的临时密码)
```
[root@izuf6anc2b2vgf95tkc5y4z ~]# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.32

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> set password for root@localhost = password('root');
Query OK, 0 rows affected, 1 warning (0.00 sec)
```
<blockquote>
<b><i>补充说明：</i></b>
<br/>第10步 报错如下
<br/>ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
<br/><b>mysql 支持 socket 和 TCP/IP 连接。那么 mysql.sock这个文件有什么用呢？
连接localhost通常通过一个Unix域套接字文件进行，一般是/tmp/mysql.sock。如果套接字文件被删除了，本地客户就不能连接。/tmp 文件夹属于临时文件，随时可能被删除。</b>
<br/>1. TCP 连接(如果报错 /tmp/mysql.sock，你可以尝试这种方式连接)
<br/>&ensp;mysql -uroot -h 127.0.0.1 -p
<br/>2. socket 连接，配置/etc/my.cnf
<br/>&ensp;[client]
<br/>&ensp;socket=/usr/local/mysql/data/mysql.sock
</blockquote>

11. 开放远程连接
```
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
msyql>update user set user.Host='%' where user.User='root';
mysql>flush privileges;
```
12. 设置开机自动启动
    1. 将服务文件拷贝到init.d下，并重命名为mysql
    >cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
    2. 赋予可执行权限
    >chmod +x /etc/init.d/mysqld
    3. 添加服务
    >chkconfig --add mysqld
    4. 显示服务列表
    >chkconfig --list
13. 参考 https://www.jianshu.com/p/276d59cbc529

## 查看Mysql版本
>[root@test02 opt]# select version();
<br/>`5.5.31-log`

## 提示too many connections，连接数不够，连接时间太长
>[root@test02 opt]# mysql
```
ERROR 1040 (HY000): Too many connections
mysql默认连接数100
```

- 修改最大连接数
>[root@test02 opt]# vi /etc/my.cnf
><br/>max_connections=1000

- 设置超时时间
>wait_timeout = 600	#最大睡眠时间,自动杀死线程
><br/>interactive_timeout = 600

- 重启mysql服务
>[root@test02 opt]# /etc/init.d/mysql restart
```
Shutting down MySQL..... SUCCESS! 
Starting MySQL.. SUCCESS! 
```

## 登录mysql终端，查看一下mysql的日志文件存放位置
>mysql> show global variables like 'log%';
```
+----------------------------------------+-----------------------+
| Variable_name                          | Value                 |
+----------------------------------------+-----------------------+
| log_bin                                | OFF                   |
| log_bin_basename                       |                       |
| log_bin_index                          |                       |
| log_bin_trust_function_creators        | OFF                   |
| log_bin_use_v1_row_events              | OFF                   |
| log_error                              | /opt/mysql/mysqld.log |
| log_output                             | FILE                  |
| log_queries_not_using_indexes          | OFF                   |
| log_slave_updates                      | OFF                   |
| log_slow_admin_statements              | OFF                   |
| log_slow_slave_statements              | OFF                   |
| log_throttle_queries_not_using_indexes | 0                     |
| log_warnings                           | 1                     |
+----------------------------------------+-----------------------+
13 rows in set (0.00 sec)
```

## MySQL Err126错误[Err] 126 - Incorrect key file for table '.\device\table_name.MYI'; try to repair it
1. 先对表进行检查，检查表命令 CHECK TABLE table_name;结果若有错误Msg_type有error修复，正常为status;
2. 对表进行修复，修复表命令 repair table table_name;
3. 再重新进行select查询即可。

## 异常问题:Error updating database.  Cause: java.sql.SQLException: Incorrect string value: '\xE2\x80\x86f\xE2\x80...' for column 'staff_id' at row 1
- 原因
	当insert数据中有表情时发生。而这些表情是按照4个字节一个单位进行编码的，而我们使用的utf-8编码在mysql数据库中默认是按照3个字节一个单位进行编码的。
- 步骤
1. 连接mysql服务
> Mysql -uroot -p root
2. 查看当前mysql数据库编码集
> SHOW VARIABLES Like '%char%';
3. 修改linux上的mysql服务配置文件mysql/my.cnf文件，添加以下配置:
```
[mysqld]
character-set-server=utf8mb4
[mysql]
default-character-set=utf8mb4
[client]
default-character-set=utf8mb4
```
4. 重启linux的mysql服务，重复第一步查看mysql编码集
> service mysql restart
- 参考
> https://www.cnblogs.com/rainerl/p/10950472.html

## 查看Mysql编码
```
mysql> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | gbk                        |
| character_set_connection | gbk                        |
| character_set_database   | gbk                        |
| character_set_filesystem | binary                     |
| character_set_results    | gbk                        |
| character_set_server     | gbk                        |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```
<br/>修改配置。my.cnf不支持character_set_results. 改为 default-character-set=utf8

## 查看表索引
> show index from tablename; 
