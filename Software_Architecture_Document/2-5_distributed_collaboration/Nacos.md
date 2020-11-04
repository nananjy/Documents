
## Nacos安装启动报错
Nacos启动报错解决：which: no javac in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)

[root@CBD-APP1 files]# which java
/usr/bin/java
[root@CBD-APP1 files]# ll /usr/bin/java
lrwxrwxrwx. 1 root root 22  8月 31 17:23 /usr/bin/java -> /etc/alternatives/java
[root@CBD-APP1 files]# ll /etc/alternatives/java
lrwxrwxrwx. 1 root root 30  8月 31 17:23 /etc/alternatives/java -> /usr/lib/jre1.8.0_152/bin/java
[root@CBD-APP1 files]# ll /usr/lib/jre1.8.0_152/
总用量 244
drwxr-xr-x.  2 root root   4096  9月  1 03:10 bin
-rw-r--r--.  1 root root   3244  8月 31 17:14 COPYRIGHT
drwxr-xr-x. 15 root root   4096  8月 31 17:14 lib
-rw-r--r--.  1 root root     40  8月 31 17:14 LICENSE
drwxr-xr-x.  4 root root   4096  8月 31 17:14 man
drwxr-xr-x.  3 root root   4096  8月 31 17:14 plugin
-rw-r--r--.  1 root root     46  8月 31 17:14 README
-rw-r--r--.  1 root root    424  8月 31 17:14 release
-rw-r--r--.  1 root root  63933  8月 31 17:14 THIRDPARTYLICENSEREADME-JAVAFX.txt
-rw-r--r--.  1 root root 145180  8月 31 17:14 THIRDPARTYLICENSEREADME.txt
-rw-r--r--.  1 root root    955  8月 31 17:14 Welcome.html
[root@CBD-APP1 files]# vi /etc/profile
在文末添加配置
# JAVA环境变量
export JAVA_HOME=/usr/lib/jre1.8.0_152/
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

[root@CBD-APP1 files]# source /etc/profile

[root@CBD-APP1 files]# sh nacos/bin/startup.sh -m standalone
/usr/lib/jre1.8.0_152//bin/java  -Xms512m -Xmx512m -Xmn256m -Dnacos.standalone=true -Dnacos.member.list= -Djava.ext.dirs=/usr/lib/jre1.8.0_152//jre/lib/ext:/usr/lib/jre1.8.0_152//lib/ext -Xloggc:/opt/files/nacos/logs/nacos_gc.log -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCTimeStamps -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M -Dloader.path=/opt/files/nacos/plugins/health,/opt/files/nacos/plugins/cmdb -Dnacos.home=/opt/files/nacos -jar /opt/files/nacos/target/nacos-server.jar  --spring.config.location=classpath:/,classpath:/config/,file:./,file:./config/,file:/opt/files/nacos/conf/ --logging.config=/opt/files/nacos/conf/nacos-logback.xml --server.max-http-header-size=524288
nacos is starting with standalone
nacos is starting，you can check the /opt/files/nacos/logs/start.out


