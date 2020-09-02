# Mysql

## 查看Mysql版本
>[root@test02 opt]# select version();
`5.5.31-log`

## 提示too many connections，连接数不够，连接时间太长
>[root@test02 opt]# mysql
`
ERROR 1040 (HY000): Too many connections
mysql默认连接数100
`
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

