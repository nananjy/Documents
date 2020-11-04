# Mysql

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
	Mysql -uroot -p root
2. 查看当前mysql数据库编码集
	SHOW VARIABLES Like '%char%';
3. 修改linux上的mysql服务配置文件
  1. 修改mysql/my.cnf文件，添加以下配置:
  ```
	[mysqld]
	character-set-server=utf8mb4

	[mysql]
	default-character-set=utf8mb4

	[client]
	default-character-set=utf8mb4
  ```
4. 重启linux的mysql服务，重复第一步查看mysql编码集
	service mysql restart
- 参考
	https://www.cnblogs.com/rainerl/p/10950472.html

