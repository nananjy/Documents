# Tomcat

### linux服务器部署 tomcat 指定jdk路径

- 修改 catalina.sh，在 脚本开头 增加 export JAVA_HOME=/usr/java/jdk1.8.0_60 设置。
```
[root@CBD-ETL-T1 jdk1.7.0_79]# vi /opt/tomcat/apache-tomcat-7.0.69/bin/catalina.sh
# OS specific support.  $var _must_ be set to either true or false.
JAVA_OPTS='-Xms512m -Xmx1024m -XX:PermSize=256M -XX:MaxPermSize=512M -Dfile.encoding=utf-8'
cygwin=false
darwin=false
os400=false
case "`uname`" in
CYGWIN*) cygwin=true;;
Darwin*) darwin=true;;
OS400*) os400=true;;
esac

##jdk路径
export JAVA_HOME=/opt/jdk1.7.0_79
```



