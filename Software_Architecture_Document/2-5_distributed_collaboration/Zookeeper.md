# Zookeeper

## 简介
ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。 ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。 在通常情况下，zookeeper允许未经授权的访问。

## 

## Zookeeper安装启动

- 启动
```
[root@CBD-WEB-T1 zookeeper-3.4.9]#  bin/zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```
- 检测是否成功启动，用zookeeper客户端连接下服务端
```
[root@CBD-WEB-T1 zookeeper-3.4.9]# bin/zkCli.sh 
Connecting to localhost:2181
2020-07-24 16:37:46,050 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.9-1757313, built on 08/23/2016 06:50 GMT
2020-07-24 16:37:46,056 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=localhost.localdomain
2020-07-24 16:37:46,056 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.8.0_91
2020-07-24 16:37:46,059 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Oracle Corporation
2020-07-24 16:37:46,059 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/home/soft/jdk/jdk1.8.0_91/jre
2020-07-24 16:37:46,059 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/home/zookeeper/zookeeper-3.4.9/bin/../build/classes:/home/zookeeper/zookeeper-3.4.9/bin/../build/lib/*.jar:/home/zookeeper/zookeeper-3.4.9/bin/../lib/slf4j-log4j12-1.6.1.jar:/home/zookeeper/zookeeper-3.4.9/bin/../lib/slf4j-api-1.6.1.jar:/home/zookeeper/zookeeper-3.4.9/bin/../lib/netty-3.10.5.Final.jar:/home/zookeeper/zookeeper-3.4.9/bin/../lib/log4j-1.2.16.jar:/home/zookeeper/zookeeper-3.4.9/bin/../lib/jline-0.9.94.jar:/home/zookeeper/zookeeper-3.4.9/bin/../zookeeper-3.4.9.jar:/home/zookeeper/zookeeper-3.4.9/bin/../src/java/lib/*.jar:/home/zookeeper/zookeeper-3.4.9/bin/../conf:.:/home/soft/jdk/jdk1.8.0_91/lib/dt.jar:/home/soft/jdk/jdk1.8.0_91/lib/tools.jar
2020-07-24 16:37:46,059 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/opt/IBM/EMM/Campaign/bin:/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
2020-07-24 16:37:46,059 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2020-07-24 16:37:46,059 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2020-07-24 16:37:46,060 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2020-07-24 16:37:46,060 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2020-07-24 16:37:46,060 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=2.6.32-131.0.15.el6.x86_64
2020-07-24 16:37:46,060 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=root
2020-07-24 16:37:46,060 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/root
2020-07-24 16:37:46,060 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/home/zookeeper/zookeeper-3.4.9
2020-07-24 16:37:46,062 [myid:] - INFO  [main:ZooKeeper@438] - Initiating client connection, connectString=localhost:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@22d8cfe0
Welcome to ZooKeeper!
2020-07-24 16:37:46,107 [myid:] - INFO  [main-SendThread(localhost.localdomain:2181):ClientCnxn$SendThread@1032] - Opening socket connection to server localhost.localdomain/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
JLine support is enabled
2020-07-24 16:37:46,255 [myid:] - INFO  [main-SendThread(localhost.localdomain:2181):ClientCnxn$SendThread@876] - Socket connection established to localhost.localdomain/127.0.0.1:2181, initiating session
[zk: localhost:2181(CONNECTING) 0] 2020-07-24 16:37:46,397 [myid:] - INFO  [main-SendThread(localhost.localdomain:2181):ClientCnxn$SendThread@1299] - Session establishment complete on server localhost.localdomain/127.0.0.1:2181, sessionid = 0x1737ff64e5b0000, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
[zk: localhost:2181(CONNECTED) 0]
```

## Zookeeper使用
- 使用 ls 命令来查看当前 ZooKeeper 中所包含的内容
>[zk: localhost:2181(CONNECTED) 1] ls /
><br/>[zookeeper]
- 创建一个新的 znode
>[zk: localhost:2181(CONNECTED) 2] create /zkPro myData
><br/>Created /zkPro
><br/>[zk: localhost:2181(CONNECTED) 3] ls /
><br/>[zookeeper, zkPro]
- 运行 get 命令查看创建的字符串
```
[zk: localhost:2181(CONNECTED) 4] get /zkPro
myData
cZxid = 0x2
ctime = Fri Jul 24 16:45:29 CST 2020
mZxid = 0x2
mtime = Fri Jul 24 16:45:29 CST 2020
pZxid = 0x2
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 6
numChildren = 0
```
- 使用 set 命令来对 zk 所关联的字符串进行设置
```
[zk: localhost:2181(CONNECTED) 5] set /zkPro myData111
cZxid = 0x2
ctime = Fri Jul 24 16:45:29 CST 2020
mZxid = 0x3
mtime = Fri Jul 24 16:50:06 CST 2020
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 9
numChildren = 0
[zk: localhost:2181(CONNECTED) 6] get /zkPro          
myData111
cZxid = 0x2
ctime = Fri Jul 24 16:45:29 CST 2020
mZxid = 0x3
mtime = Fri Jul 24 16:50:06 CST 2020
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 9
numChildren = 0
```
- 删除 znode
```
[zk: localhost:2181(CONNECTED) 8] delete /zkPro       
[zk: localhost:2181(CONNECTED) 9] ls /
[zookeeper]
```
- 退出
```
[zk: localhost:2181(CONNECTED) 6] quit
Quitting...
2020-07-27 10:45:29,981 [myid:] - INFO  [main:ZooKeeper@684] - Session: 0x1737ff64e5b0002 closed
2020-07-27 10:45:29,983 [myid:] - INFO  [main-EventThread:ClientCnxn$EventThread@519] - EventThread shut down for session: 0x1737ff64e5b0002
```

## 使用JavaAPI操作zookeeper
1. 在zookeeper里增加一个目录节点，并且把配置信息存储在里面
```
[zk: localhost:2181(CONNECTED) 4] create /username wzy
wzy
cZxid = 0x5
ctime = Fri Jul 24 17:41:53 CST 2020
mZxid = 0x5
mtime = Fri Jul 24 17:41:53 CST 2020
pZxid = 0x5
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 3
numChildren = 0
```
2. 启动两个zookeeper客户端程序
```
package com.example.demo;
import java.util.concurrent.CountDownLatch;
import org.apache.zookeeper.WatchedEvent;
import org.apache.zookeeper.Watcher;
import org.apache.zookeeper.Watcher.Event.EventType;
import org.apache.zookeeper.Watcher.Event.KeeperState;
import org.apache.zookeeper.ZooKeeper;
import org.apache.zookeeper.data.Stat;

/**
 * 分布式配置中心demo
 */

public class ZookeeperTest implements Watcher {

    private static CountDownLatch connectedSemaphore = new CountDownLatch(1);
    private static ZooKeeper zk = null;
    private static Stat stat = new Stat();

    public static void main(String[] args) throws Exception {

        //zookeeper配置数据存放路径
        String path = "/username";
        //连接zookeeper并且注册一个默认的监听器
        zk = new ZooKeeper("172.26.128.143:2181", 5000, //
                new ZookeeperTest());
        //等待zk连接成功的通知
        connectedSemaphore.await();
        //获取path目录节点的配置数据，并注册默认的监听器
        System.out.println(new String(zk.getData(path, true, stat)));
        Thread.sleep(Integer.MAX_VALUE);
    }

    public void process(WatchedEvent event) {

        if (KeeperState.SyncConnected == event.getState()) {  //zk连接成功通知事件
            if (EventType.None == event.getType() && null == event.getPath()) {
                connectedSemaphore.countDown();
            } else if (event.getType() == EventType.NodeDataChanged) {  //zk目录节点数据变化通知事件
                try {
                    System.out.println("配置已修改，新值为：" + new String(zk.getData(event.getPath(), true, stat)));
                } catch (Exception e) {
                }
            }
        }
    }
}
```

3. 我们在zookeeper里修改下目录节点/username下的数据
```
[zk: localhost:2181(CONNECTED) 3] set /username aici     
cZxid = 0x5
ctime = Fri Jul 24 17:41:53 CST 2020
mZxid = 0x9
mtime = Mon Jul 27 10:33:59 CST 2020
pZxid = 0x5
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
```
>执行结果
```
"C:\Program Files\Java\jdk1.8.0_73\bin\java.exe" -javaagent:D:\java-applications\IDEA\lib\idea_rt.jar=59338:D:\java-applications\IDEA\bin -Dfile.encoding=UTF-8 -classpath "...jar;" com.example.demo.ZookeeperTest
log4j:WARN No appenders could be found for logger (org.apache.zookeeper.ZooKeeper).
log4j:WARN Please initialize the log4j system properly.
wzy
配置已修改，新值为：aici
```

## Zookeeper集群模式安装
1. 配置JAVA环境，检查环境
```
[root@CBD-WEB-T1 zookeeper-3.4.9]# java -version
java version "1.8.0_91"
Java(TM) SE Runtime Environment (build 1.8.0_91-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b14, mixed mode)
```
2. 修改配置文件zoo-1.cfg，原配置文件里有的，修改成下面的值，没有的则加上
```
[root@CBD-WEB-T1 zookeeper-3.4.9]# less conf/zoo-1.cfg 
# The number of milliseconds of each tick
tickTime=2000								#服务器之间或客户端与服务器之间维持心跳的间隔
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10								#Zookeeper服务器集群中连接到Leader的Follower服务器初始化连接时最长忍受的心跳数
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5									#Leader与Follower之间发送消息，请求和应答时间长度
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/tmp/zookeeper-1 						#保存数据、日志的目录
# the port at which the clients will connect
clientPort=2181 							#客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口，接受客户端的访问请求
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60 							#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
server.1=127.0.0.1:2888:3888
server.2=127.0.0.1:2889:3889
server.3=127.0.0.1:2890:3890

server.A=B：C：D：其中 A 是一个数字，表示这个是第几号服务器；B 是这个服务器的 ip 地址；C 表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；D 表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。如果是伪集群的配置方式，由于 B 都是一样，所以不同的 Zookeeper 实例通信端口号不能一样，所以要给它们分配不同的端口号。
```
3. 从zoo-1.cfg复制两个配置文件zoo-2.cfg和zoo-3.cfg，只需修改dataDir和clientPort不同即可
```
[root@CBD-WEB-T1 conf]# cp zoo-1.cfg zoo-2.cfg 
[root@CBD-WEB-T1 conf]# vi zoo-2.cfg 
dataDir=/tmp/zookeeper-2
clientPort=2182
[root@CBD-WEB-T1 conf]# cp zoo-1.cfg zoo-3.cfg
dataDir=/tmp/zookeeper-3
clientPort=2183
```
4. 标识ServerID
```
[root@CBD-WEB-T1 conf]# vi /tmp/zookeeper-1/myid
1
[root@CBD-WEB-T1 conf]# vi /tmp/zookeeper-2/myid
2
[root@CBD-WEB-T1 conf]# vi /tmp/zookeeper-3/myid
3
```
5. 启动三个zookeeper实例
```
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh start zoo-1.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo-1.cfg
Starting zookeeper ... STARTED
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh start zoo-2.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo-2.cfg
Starting zookeeper ... STARTED
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh start zoo-3.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo-3.cfg
Starting zookeeper ... STARTED
```
6. 检测集群状态，也可以直接用命令“zkCli.sh -server IP:PORT”连接zookeeper服务端检测
```
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh status zoo-1.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo-1.cfg
Mode: follower
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh status zoo-2.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo-2.cfg
Mode: follower
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh status zoo-3.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo-3.cfg
Mode: leader
```
7. 停止zookeeper实例
```
[root@CBD-WEB-T1 conf]# ../bin/zkServer.sh stop zoo.cfg 
ZooKeeper JMX enabled by default
Using config: /home/zookeeper/zookeeper-3.4.9/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
```

## Zookeeper2181端口未授权访问iptables编写规则
- 查看当前规则
```
[root@CBD-Hadoop-DataNode22 ~]# iptables -nL --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     tcp  --  10.68.32.49          0.0.0.0/0           tcp dpt:2181 
2    ACCEPT     tcp  --  10.68.32.50          0.0.0.0/0           tcp dpt:2181 
3    ACCEPT     tcp  --  10.68.32.51          0.0.0.0/0           tcp dpt:2181 
4    DROP       tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:2181 

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination         
-- 查看详细信息
[root@CBD-Hadoop-DataNode24 ~]# iptables -nvL --line-number
Chain INPUT (policy ACCEPT 23274 packets, 6315K bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1       98  5651 ACCEPT     tcp  --  *      *       10.68.32.51          0.0.0.0/0           tcp dpt:2181 
2        0     0 ACCEPT     tcp  --  *      *       10.68.32.49          0.0.0.0/0           tcp dpt:2181 
3        0     0 ACCEPT     tcp  --  *      *       10.68.32.50          0.0.0.0/0           tcp dpt:2181 
4        0     0 DROP       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0           tcp dpt:2181 

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 16462 packets, 1632K bytes)
num   pkts bytes target     prot opt in     out     source               destination      
```
- **添加**到规则末尾
>[root@CBD-Hadoop-DataNode22 ~]# iptables -A INPUT -s 10.68.32.51 -p tcp --dport 2181 -j ACCEPT
- 插入到规则指定位置，默认插入到规则首部
>[root@CBD-Hadoop-DataNode22 ~]# iptables -I INPUT -s 10.68.32.49 -p tcp --dport 2181 -j ACCEPT
><br/>[root@CBD-Hadoop-DataNode24 ~]# iptables -I INPUT 2 -s 10.68.32.49 -p tcp --dport 2181 -j ACCEPT
- **删除**第二行规则
>[root@CBD-Hadoop-DataNode22 ~]# iptables -D INPUT 2
- 测试网络关系
```
[root@CBD-Hadoop-DataNode23 ~]# telnet 10.68.32.29 2181
Trying 10.68.32.29...
Connected to 10.68.32.29.
Escape character is '^]'.
^CConnection closed by foreign host.
```
- **修改**第四条规则为ACCEPT
>[root@CBD-Hadoop-DataNode23 ~]# iptables -R INPUT 4 -j ACCEPT
-- 配置生效，启动情况下无需重启
>service iptables save
><br/>service iptables restart
- iptables命令使用详解(https://www.cnblogs.com/vathe/p/6973656.html) 

