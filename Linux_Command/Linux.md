# Linux

### Linux文件删除，但是df之后磁盘空间没有释放 
Linux 磁盘空间总是报警，查到到大文件，删除之后，df看到磁盘空间并没有释放。
查找了下发现系统对rm进行了alias，因为Linux对删除操作没有回收站机制，对rm操作进行了自定义，对删除文件进行移动到/tmp目录里面。
执行   lsof | grep deleted发现有大量刚刚删除文件的进程存在，kill掉进程（或者重启进程）OK

- 清空日志文件内容
>[root@cbd-app-22-vm ~]# echo "" > /opt/apache-tomcat-6.0.47/logs/catalina.out

- 查看文件系统占用
```
[root@cbd-cloud-01-vm ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/rootvg-root   21G  8.9G   13G  43% /
devtmpfs                 7.9G     0  7.9G   0% /dev
tmpfs                    7.9G     0  7.9G   0% /dev/shm
tmpfs                    7.9G  833M  7.1G  11% /run
tmpfs                    7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/sda1               1014M  189M  826M  19% /boot
/dev/mapper/rootvg-home  5.0G   33M  5.0G   1% /home
/dev/mapper/rootvg-opt   150G  150G   20K 100% /opt
/dev/mapper/rootvg-var    15G  341M   15G   3% /var
tmpfs                    1.6G     0  1.6G   0% /run/user/0
```
- 查看磁盘节点挂载占用 
```
[root@cbd-cloud-02-vm opt]# df -ih
Filesystem              Inodes IUsed IFree IUse% Mounted on
/dev/mapper/rootvg-root   142K  141K  1.1K  100% /
devtmpfs                  2.0M   389  2.0M    1% /dev
tmpfs                     2.0M     1  2.0M    1% /dev/shm
tmpfs                     2.0M   524  2.0M    1% /run
tmpfs                     2.0M    16  2.0M    1% /sys/fs/cgroup
/dev/sda1                 512K   326  512K    1% /boot
/dev/mapper/rootvg-opt     75M   897   75M    1% /opt
/dev/mapper/rootvg-var    7.5M  5.4K  7.5M    1% /var
/dev/mapper/rootvg-home   2.5M    29  2.5M    1% /home
tmpfs                     2.0M     1  2.0M    1% /run/user/0
```
- 查看当前目录下每个文件大小
```
[root@cbd-cloud-02-vm opt]# du -sh *
712K    12.txt
74M     apache-tomcat-8.0.50
0       applogs
29M     bak
40M     cbd
35M     cbd_eureka.tar.gz
4.0K    files
0       mpms
0       mpms_bak
0       rh
334M    rizhiyi
32K     RZY
```
- 查看deleted的进程
```
[root@cbd-cloud-01-vm ~]# lsof | grep deleted
cupsd      1022                 root   10r      REG              253,0         2123    2365941 /etc/passwd+ (deleted)
java      48247                 root    1w      REG              253,4 160074420224        109 /opt/apache-tomcat-8.0.50/logs/catalina.out (deleted)
```
- 杀掉deleted进程
```
[root@cbd-cloud-01-vm ~]# kill -9 48247
[root@cbd-cloud-01-vm ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/rootvg-root   21G  7.3G   14G  35% /
devtmpfs                 7.9G     0  7.9G   0% /dev
tmpfs                    7.9G     0  7.9G   0% /dev/shm
tmpfs                    7.9G  833M  7.1G  11% /run
tmpfs                    7.9G     0  7.9G   0% /sys/fs/cgroup
/dev/sda1               1014M  189M  826M  19% /boot
/dev/mapper/rootvg-home  5.0G   33M  5.0G   1% /home
/dev/mapper/rootvg-opt   150G  866M  150G   1% /opt
/dev/mapper/rootvg-var    15G  341M   15G   3% /var
tmpfs                    1.6G     0  1.6G   0% /run/user/0
```
- 重启进程，上面删除日志会同时杀掉应用，注意重启
```
[root@cbd-cloud-02-vm bin]# sh startup.sh
Using CATALINA_BASE:   /opt/apache-tomcat-8.0.50
Using CATALINA_HOME:   /opt/apache-tomcat-8.0.50
Using CATALINA_TMPDIR: /opt/apache-tomcat-8.0.50/temp
Using JRE_HOME:        /usr
Using CLASSPATH:       /opt/apache-tomcat-8.0.50/bin/bootstrap.jar:/opt/apache-tomcat-8.0.50/bin/tomcat-juli.jar
Tomcat started.
```
- 参考：https://www.cnblogs.com/xd502djj/p/6668632.html

### linux查看路由的三种方式
- route
- netstat -route
- ip route

### linux设置系统时间
- 手动修改
1. 查看当前时间
> [root@localhost opt]# date
<br/>Thu Dec 10 20:43:51 CST 2020
2. 修改时间
> date -s 时分秒
3. 修改日期、时间
> [root@localhost opt]# date -s '2020-12-11 14:34:50'
<br/>date -s 完整日期时间（YYYY-MM-DD hh:mm[:ss]），时间要用双引号括起来，否则报错
4. 将时间写入bios避免重启失效
> [root@localhost opt]# hwclock -w
<br/>当我们进行完 Linux 时间的校时后，还需要以 hwclock 来更新 BIOS 的时间，因为每次重新启动的时候，系统会重新由 BIOS 将时间读出来，所以，BIOS才是重要的时间依据吶。

- 同步网络时间
1. 检查系统是否安装ntp服务
> apt-get install ntp  或者 yum install ntp
2. 检查ntp服务是否启动
> service --status-all
3. ntpdate 服务器IP：同步服务器时间
```
ntp常用服务器：
    中国国家授时中心：210.72.145.44
    NTP服务器(上海) ：ntp.api.bz
    美国：time.nist.gov 
    复旦：ntp.fudan.edu.cn 
    微软公司授时主机(美国) ：time.windows.com 
    台警大授时中心(台湾)：asia.pool.ntp.org
```
4. 修改服务器时区
> date -R ： 查看当前时区
<br/> tzselect
<br/> cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 时区没改回来

### linux查看所有服务
> service --status-all

### 挂载光盘出现写保护mount: block device /dev/sr0 is write-protected, mounting read-only
> [root@localhost /]# mount -o loop /opt/dvd.iso /mnt 
<br/>mount: block device /dev/sr0 is write-protected, mounting read-only 
<br/>虚拟机挂载光驱时提示只读，用以下命令可解决该报错 
<br/>mount -o remount,rw /dev/cdrom /mnt/cdrom



