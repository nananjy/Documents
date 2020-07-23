# Oracle

## Oracle表空间

- Oracle创建表空间
>create tablespace ycm logging datafile 'D:\oracleData\ycm.dbf' size 50m autoextend on next 50m maxsize 1024m extent management local;

- Oracle创建临时表空间
>create temporary tablespace ycm_temp tempfile 'D:\oracleData\ycm_temp.dbf' size 50m autoextend on next 50m maxsize 1024m extent management local;

- Oracle删除表空间
   - 删除空的表空间，但是不包含物理文件
>drop tablespace tablespace_name;
   - 删除非空表空间，包含物理文件
?drop tablespace tablespace_name including contents and datafiles;
>注意事项：
><br/>如果drop tablespace语句中含有datafiles,那datafiles之前必须有contents关键字,不然会提示ora-01911错误

   - 如果其他表空间中的表有外键等约束关联到了本表空间中的表的字段，就要加上cascade constraint
>drop tablespace tablespace_name including contents and datafiles cascade constraint;

- Oracle删除数据文件
>报错ORA-01157: 无法标识/锁定数据文件 12 - 请参阅 DBWR 跟踪文件。ORA-01110: 数据文件TSTEST001.DBF。原因：数据文件在没有被offline的情况下物理删除了,导致oracle的数据不一致,因此启动失败.解决如下：
><br/>alter database datafile 'E:\ORACLE\PRODUCT\10.2.0\ORADATA\ORCL\TSTEST001.DBF' offline drop; 
><br/>alter database open; 
><br/>drop tablespace CTBASEDATA; 
- Oracle把表移动到其它表空间
>alter table test move tablespace PERFSTAT;

- 表空间与数据文件脱机区别


## Oracle用户

- 创建用户
>create user ycm identified by motorXMD312 default tablespace ycm temporary tablespace ycm_temp;

- 查找用户
>select *　from dba_users;

- 查找工作空间的路径
>select * from dba_data_files; 

- 删除用户及级联关系
>drop user 用户名称 cascade;

删除无任何数据对象的表空间：
首先使用PL/SQL界面化工具，或者使用oracle自带的SQL PLUS工具，连接需要删除的表空间的oracle数据局库。
确认当前用户是否有删除表空间的权限，如果没有 drop tablespace,请先用更高级的用户(如sys)给予授权或者直接用更高级的用户。
用drop tablespace xxx ，删除需要删除的表空间。
删除有任何数据对象的表空间
使用drop tablespace xxx including contents and datafiles;来删除表空间。






