# Oracle

* [Oracle安装](#tag1)
  * [配置账户、目录、环境变量](#tag1-1)
  * [配置VNC](#tag1-2)
* [Oracle表空间](#tag2)

## <span id = "tag1">Oracle安装</span>

### <span id = "tag1-1">配置账户、目录、环境变量</span>
- 上传oracle包，linux6.1包(用于配置yum源，安装可视化桌面）
```
-rw-r--r--. 1 root root      58300 Aug 12 15:41 ftp-0.17-51.1.el6.x86_64.rpm
-rw-------. 1 root root 1239269270 Aug 12 16:36 linux.x64_11gR2_database_1of2.zip
-rw-r--r--. 1 root root 1111416131 Aug 12 16:37 linux.x64_11gR2_database_2of2.zip
-rw-r--r--. 1 root root      72540 Aug 12 15:41 lrzsz-0.12.20-27.1.el6.x86_64.rpm
-rw-r--r--. 1 root root     187796 Aug 12 15:41 rdesktop-1.8.2-1.el7.nux.x86_64.rpm
-rw-r--r--. 1 root root     178464 Aug 12 15:41 sysstat-7.0.2-3.el5.x86_64.rpm
-rw-r--r--. 1 root root      59084 Aug 12 15:41 telnet-0.17-46.el6.x86_64.rpm
```
- 配置oracle账户
```
[root@test01 rpm_zip]# groupadd dba
[root@test01 rpm_zip]# groupadd oinstall
[root@test01 rpm_zip]# less /etc/group
dba:x:501:
oinstall:x:502:
[root@test01 rpm_zip]# useradd oracle
[root@test01 rpm_zip]# less /etc/passwd
oracle:x:500:503::/home/oracle:/bin/bash
	7 个字段的详细信息如下:
	用户名 （magesh）： 已创建用户的用户名，字符长度 1 个到 12 个字符。
	密码（x）：代表加密密码保存在 `/etc/shadow 文件中。
	用户ID（506）：代表用户的ID号，每个用户都要有一个唯一的UID, UID号为 0 的是为root用户保留的，UID号 1-99 是为系统用户保留的，UID号 100-999 是为系统账户和群组保留的。
	群组ID（507）：代表群组的ID号，每个群组都要有一个唯一的GID ，保存在 /etc/group文件中。
	用户信息（2g Admin - Magesh M）：代表描述字段，可以用来描述用户的信息（LCTT 译注：此处原文疑有误）。
	家目录（/home/mageshm）：代表用户的家目录。
	Shell（/bin/bash）：代表用户使用的 shell 类型。
[root@test01 rpm_zip]# usermod -g oinstall -G dba oracle
oracle:x:500:502::/home/oracle:/bin/bash
(或者直接)useradd -g oinstall -G dba oracle
[root@test01 rpm_zip]# passwd oracle
passwd: all authentication tokens updated successfully.
```
- 预先创建安装目录
```
[root@test01 rpm_zip]# mkdir -p /opt/oracle/product
[root@test01 rpm_zip]# mkdir -p /opt/oracle/product/OraHome
[root@test01 rpm_zip]# mkdir -p /opt/oraInventory
[root@test01 rpm_zip]# mkdir -p /data/oracle/oradata
```
- 设置目录的所有者所属组和权限
```
[root@test01 opt]# chown -R oracle.oinstall /opt/oracle
#[root@test01 product]# chown -R oracle.oinstall /opt/oracle/product/OraHome
[root@test01 opt]# chown -R oracle.oinstall /data/oracle/oradata
[root@test01 opt]# chown -R oracle.dba /opt/oraInventory
[root@test01 opt]# chmod -R 775 /opt/oracle
[root@test01 opt]# ll
drwxrwxr-x. 3 oracle oinstall  4096 Aug 14 11:37 oracle
drwxr-xr-x. 2 oracle dba       4096 Aug 14 11:27 oraInventory
```
- 设置用户oracle的环境变量
```
[root@test01 opt]# su - oracle
[oracle@test01 ~]$ vi /home/oracle/.bash_profile
在文件中添加如下：
export ORACLE_BASE=/opt/oracle
export ORACLE_HOME=$ORACLE_BASE/product/OraHome
export ORACLE_SID=tmporadb
export ORACLE_OWNER=oracle
export ORACLE_TERM=vt100
export PATH=$PATH:$ORACLE_HOME/bin:$HOME/bin
export PATH=$ORACLE_HOME/bin:$ORACLE_HOME/Apache/Apache/bin:$PATH
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib:/usr/local/lib
export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
CLASSPATH=$CLASSPATH:$ORACLE_HOME/network/jlib
export CLASSPATH
PATH=$PATH:/usr/sbin; export PATH
PATH=$PATH:/usr/bin; export PATH
注意：
11g：ORA_NLS33=$ORACLE_HOME/nls/admin/data
10g：ORA_NLS33=$ORACLE_HOME/ocommon/nls/admin/data
9i： ORA_NLS33=/oracle/app/ora92/ocommon/nls/admin/data
保存退出
```
- 执行以下命令让配置马上生效或以oracle用户登录使设置生效
>source $HOME/.bash_profile
- 

### <span id = "tag1-2">配置VNC</span>
- 首次安装配置vncserver，需输入两次密码zzy123
```
[root@test01 ~]# vncserver
You will require a password to access your desktops.
Password:
Password must be at least 6 characters - try again
Password:
Verify:
New 'test01:1 (root)' desktop is test01:1
Creating default startup script /root/.vnc/xstartup
Starting applications specified in /root/.vnc/xstartup
Log file is /root/.vnc/test01:1.log
```
- 编辑配置文件
```
[root@test01 ~]# vi .vnc/xstartup
#!/bin/sh

[ -r /etc/sysconfig/i18n ] && . /etc/sysconfig/i18n
export LANG
export SYSFONT
vncconfig -iconic &
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
OS=`uname -s`
if [ $OS = 'Linux' ]; then
  case "$WINDOWMANAGER" in
    *gnome*)
      if [ -e /etc/SuSE-release ]; then
        PATH=$PATH:/opt/gnome/bin
        export PATH
      fi
      ;;
  esac
fi
if [ -x /etc/X11/xinit/xinitrc ]; then
  exec /etc/X11/xinit/xinitrc
fi
if [ -f /etc/X11/xinit/xinitrc ]; then
  exec sh /etc/X11/xinit/xinitrc
fi
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
twm &
```

Vnc viewer连接超时
服务器打开了 vncserver但是vnc viewer无法连接，连接超时。
原因：服务器打开了防火墙需要手工开启相应的端口号：
[root@localhost~]# iptables -I INPUT -p tcp --dport 5901 -j ACCEPT    #
- linux下执行xhost +命令进入图形界面报错：unable to open display
> 执行export DISPLAY=localhost:1 使得所有客户都可以访问
><br/> xhost + ip 指定ip机器可以使用该服务

### Oracle运行安装 
[oracle@test01 database]$ ./runInstaller
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 17084 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 4095 MB    Passed
Checking monitor: must be configured to display at least 256 colors.    Actual 16777216    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2020-08-24_02-58-05PM. Please wait ...[oracle@test01 database]$

### Swap报错解决
https://blog.csdn.net/hzh839900/article/details/79215703
### Hard Limit max file
https://blog.csdn.net/andyguan01_2/article/details/86681505
### Soft Limit max user processes
https://www.cnblogs.com/rusking/p/4388589.html
### kernel 报错
https://blog.csdn.net/benbenzhuhwp/article/details/45013587?utm_source=blogxgwz8
### Centos6.9升级glibc解决“libc.so.6: version GLIBC_2.14 not found”报错问题
https://blog.csdn.net/heylun/article/details/78833050?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase
### 问题列表
1. 依赖包检查失败， Centos7 上安装的依赖包要比 oracle 11g 所需要的版本更高，可以直接忽略。
2. semmni 检查失败， sysctl.con f 里配置的 semmni 是 4096 ，远大于 128 ，但是检查 semmni 提示是0，忽略。
3. 安装到84%报Error in invoking target ‘ install ’  of makefile '/u01/app/oracle/product/11.2.0/dbhome_1/ctx/lib/ins_ctx.mk'
   原因：看日志缺少32 位相关依赖包。
   解决：修改/u01/app/oracle/product/11.2.0/dbhome_1/ctx/lib/ins_ctx.mk，将 
    ctxhx: $(CTXHXOBJ) 
          $(LINK_CTXHX) $(CTXHXOBJ) $(INSO_LINK) 
   修改为： 
    ctxhx: $(CTXHXOBJ) 
          -static $(LINK_CTXHX) $(CTXHXOBJ) $(INSO_LINK) /usr/lib64/stdc.a
   点击Retry继续安装
4. 接着报Error in invoking target 'agent nmhs' of makefile '/u01/app/oracle/product/11.2.0/dbhome_1/sysman/lib/ins_emagent.mk.'
   解决：在makefile中添加链接libnnz11库的参数 
   修改/u01/app/oracle/product/11.2.0/dbhome_1/sysman/lib/ins_emagent.mk，将 
    $(MK_EMAGENT_NMECTL)修改为：$(MK_EMAGENT_NMECTL) -lnnz11 
   点击Retry继续安装。 其中：-lnnz 和 $(MK_EMAGENT_NMECTL) 之间有空格

### root执行，配置权限
```
[root@test01 oraInventory]# ./orainstRoot.sh                            
Changing permissions of /opt/oraInventory.                              
Adding read,write permissions for group.                                
Removing read,write,execute permissions for world.                      

Changing groupname of /opt/oraInventory to dba.
The execution of the script is complete.  
```
### 配置环境变量
```
[root@test01 OraHome]# ./root.sh
Running Oracle 11g root.sh script...

The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /opt/oracle/product/OraHome

Enter the full pathname of the local bin directory: [/usr/local/bin]: /usr/local/bin
   Copying dbhome to /usr/local/bin ...
   Copying oraenv to /usr/local/bin ...
   Copying coraenv to /usr/local/bin ...

Creating /etc/oratab file...
Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root.sh script.
Now product-specific root actions will be performed.
Finished product-specific root actions.
```
### 监听配置
```
[oracle@test01 database]$ netca
Oracle Net Services Configuration:
Configuring Listener:LISTENER
Listener configuration complete.
Oracle Net Listener Startup:
    Running Listener Control:
      /opt/oracle/product/OraHome/bin/lsnrctl start LISTENER
    Listener Control complete.
    Listener started successfully.
Oracle Net Services configuration successful. The exit code is 0
```
### 安装数据库程序

### 安装RPM包
```
安装rpm包
sudo rpm -Uvh *-2.17-55.el6.x86_64.rpm --force --nodeps

/etc/cron.d is needed by sysstat-9.0.4-27.el6.x86_64
原因：
这是由于yum初始化安装时，安装了旧版本的GPG keys造成的
[root@leader Packages]# rpm --import /etc/pki/rpm-gpg/RPM*
再次安装rpm包时，后面加上--force --nodeps如
[root@leader Packages]# rpm -ivh sysstat* --force --nodeps

unixODBC-devel-2.2.11 (i386) 缺少
unixODBC-devel-2.2.14-11.el6(x86_64) 已经安装
yum list libaio-devel 查看下名字
之后yum install libaio-devel-0.3.105.i386

如果已经安装了该库，可以尝试rpm强制安装
libc.so.6(GLIBC_2.14)(64bit) is needed by问题 
已经安装glibc2.14，rpm安装还是报错
```
### vncserver 配置
https://jingyan.baidu.com/article/92255446a89400851648f4b9.html

### xhost access control disabled, clients can connect from any host
https://blog.csdn.net/yabingshi_tech/article/details/8032076
### 安装oracle 11g过程中，prerequisite checks过程中各种failed问题解决
https://blog.csdn.net/benbenzhuhwp/article/details/45013587?utm_source=blogxgwz8

### Oracle常用系统表
https://www.cnblogs.com/mengzhixingping/p/5085410.html

### Oracle的RPM文件下载路径
https://pkgs.org/search/?q=pdksh



## <a name = "tag2">Oracle表空间</a>

- Oracle创建表空间
>create tablespace ycm logging datafile 'D:\oracleData\ycm.dbf' size 50m autoextend on next 50m maxsize 1024m extent management local;

- Oracle创建临时表空间
>create temporary tablespace ycm_temp tempfile 'D:\oracleData\ycm_temp.dbf' size 50m autoextend on next 50m maxsize 1024m extent management local;

- Oracle删除表空间
   - 删除空的表空间，但是不包含物理文件
   >drop tablespace tablespace_name;
   - 删除非空表空间，包含物理文件
   >drop tablespace tablespace_name including contents and datafiles;
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

- 删除用户
>drop user ycm;
><br/>若用户拥有对象，则不能直接删除，否则将返回一个错误值。指定关键字cascade,可删除用户所有的对象，然后再删除用户。
><br/>drop user ycm cascade;

- 查找工作空间的路径
>select * from dba_data_files; 

- 删除用户及级联关系
>drop user 用户名称 cascade;

- 查看用户或角色系统权限(直接赋值给用户或角色的系统权限)
>select * from dba_sys_privs;

- 查看哪些用户有sysdba或sysoper系统权限(查询时需要相应权限)
>select * from V$PWFILE_USERS;

- 登录Oracle
>sqlplus /nolog
><br/>conn / as sysdba

- 授予权限
>grant create session to ycm;--连接数据库
><br/>grant unlimited tablespace to 用户名;--操作表空间
><br/>grant create table to 用户名;--创建表
><br/>grante drop table to 用户名;--删除表
><br/>grant insert table to 用户名;--插入表
><br/>grant update table to 用户名;--更新表
><br/>connect(只对其他用户的表有访问权限，包括select/insert/update和delete等，还能够创建表、视图、序列(sequence)、簇(cluster)、同义词(synonym)、回话(session)和其他  数据的链(link))
><br/>resource(给用户另外的权限以创建他们自己的表、序列、过程(procedure)、触发器(trigger)、索引(index)和簇(cluster))
><br/>dba(系统权限，包括无限制的空间限额和给其他用户授予各种权限的能力。system由dba用户拥有)

- 撤销权限
>revoke connect, resource from ycm;

- 创建/授权/删除角色
>用户创建的role可以由表或系统权限或两者的组合构成。
><br/>create role test;--创建角色
><br/>grant select on class to test;--拥有testRole角色的所有用户都具有对class表的select查询权限
><br/>drop role testRole;--与testRole角色相关的权限将从数据库全部删除

## Oracle连接数
- 查看不用用户连接数
>select username,count(username) from v$session where username is not null group by username;
><br/>select count(*) from v$process --当前的连接数*
><br/>select value from v$parameter where name = 'processes' --数据库允许的最大连接数

- 修改最大连接数
>alter system set processes = 300 scope = spfile;

- 重启数据库
>shutdown immediate;
><br/>startup;
   - 启动报错LRM-00109: could not open parameter file '/u01/oracle/product/OraHome/dbs/inithbdb.ora'
   ```
    你的ORACLE_SID参数有问题，三个地方的SID可以查一下是否一致：
    1、$ORACLE_BASE/admin/SID_NAME/pfile文件夹下的init文件中的SID;
    2、/etc/oratab中的最回后一行答第一个":"前，如"oracl:/u01/app/oracle/product/11.2.0/dbhome_1:N"中的"oracl";
    3、.bash_profile中的SID;
    先startup，再conn。
   ```
   - 报错ERROR at line 1:ORA-01034: ORACLE not available
   解决：先测试一下，监听是否启动：lsnrctl status;然后输入startup，启动oracle。
   - 下列错误，是非正常关闭导致LOG损坏
   ```
    ORA-16038: log 2 sequence# 1 cannot be archived 
    ORA-19809: limit exceeded for recovery files 
    ORA-00312: online log 2 thread 1: '/home/oracle/oradata/orc2/redo02.log' 
    需进行不完全恢复 
    SQL>startup mount 
    SQL>recover database until cancel 
    SQL>alter database open resetlogs;
   ```

## Oracle数据回滚:基于时间的查询（AS OF TIMESTAMP）
>create table informationlaw_bak as
><br/>select * from informationlaw as of TIMESTAMP to_timestamp('20121126 103435','yyyymmdd hh24miss');

## Oracle数据库导入导出
- expdp 服务器端导出工具
>不可以运行于Oracle客户端，derectory目录OS须提前建好，

- impdp 服务器端导入工具

## Oracle一次性插入多条数据操作
- 问题：每次使用需要向数据库插入很多数据，导致页面等待很长时间才有结果。
  id：采用sequence自增
  每次循环，都会查询一次sequence，然后insert一条数据，性能非常低。
- 解决
> 在oracle里面，不支持像mysql那样直接在后面拼多个记录。
```
INSERT INTO `table_data` VALUES 
(50, '2020-11-1', 'success', 'Y'), 
(51, '2020-11-2', 'success', 'Y'); 
```
oracle中有两种方法达到批量插入的效果
1. 采用union all拼接查询方式
```
INSERT INTO "table_data" 
SELECT 'CSXM201609F00190', 'keyword5' FROM dual
UNION ALL 
SELECT 'CSXM201609F00190', 'keyword6' FROM dual
UNION ALL 
SELECT 'CSXM201609F00190', 'keyword7' FROM dual; 
```
2. 采用insert all的方式，insert all还支持往不同的表里插入数据
```
通过sequence获取的值是同一个，采用触发器方式自动设置id。
-- 创建序列
create sequence seq_table 
minvalue 1
maxvalue 999999999999999999999999
start with 1
increment by 1
cache 20;
-- 创建触发器
create or replace trigger tr_test_insert
before insert on test_insert
for each row
begin
  select seq_ttable.nextval into :new.id from dual;
end;
-- 插入数据
insert all 
into table(name,age) values('aaa','8')
into table(name,age) values('bbb','2')
into table(name,age) values('ccc','4')
select * from dual;
```

## Root账号跳转到Oracle账号，执行sqlplus报错
- su oracle 和 su - oracle的区别
```
-, -l ,–login make the shell a login shell
加了‘-’，是以login shell登陆的，所以会设置环境变量，如果不加，使用的还是切换前用户的环境变量，报错。
```

## Oracle误操作更新表记录后恢复（使用as of timestamp）
1. 误操作指令，没有加where
> update TBL_SCM_ACT_COM_TASK set com_status_cd = '8' 
2. 查看指定时间点操作表的数据
> select * from TBL_SCM_ACT_COM_TASK as of timestamp to_timestamp('2010-06-02 11:36:53','yyyy-mm-dd hh24:mi:ss');
3. 找到数据，批量插入
> update TBL_SCM_ACT_COM_TASK t1 
<br/> set t1.com_status_cd= 
<br/> (select com_status_cd
<br/> from TBL_SCM_ACT_COM_TASK  as of timestamp sysdate - 20/1440 
<br/> where t1.act_com_task_id = act_com_task_id) ; 
4. FLASHBACK TABLE test TO TIMESTAMP TIMESTAMP '2010-3-18 10:00:00'; 这要求TEST表事先有ENABLE ROW MOVEMENT
5. 误更新或者删除
> alter table ch_t_song_info enable row movement;
<br/> flashback table ch_t_song_info to timestamp to_timestamp('2013-11-27 14:00:00','yyyy-mm-dd hh24:mi:ss');
6. 误drop
> flashback table ch_t_song_info to before update

