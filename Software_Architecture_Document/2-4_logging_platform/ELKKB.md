# ELK

## ELK日志系统：Elasticsearch + Logstash + Kibana 搭建流程
- Logstash(收集服务器上的日志文件) --》然后保存到 ElasticSearch（搜索引擎） --》Kibana提供友好的web界面（从ElasticSearch读取数据进行展示）


## 安装教程

关闭防火墙
查看防火墙状态：systemctl status firewalld.service
关闭防火墙：systemctl stop firewalld.service
禁止开机启动防火墙：systemctl disable firewalld.service

### 安装报错
1. max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
修改/etc/security/limits.conf文件，增加配置，用户退出后重新登录生效
*  soft    nofile          65536
*  hard    nofile          131072
*  soft    memlock         unlimited
*  hard    memlock         unlimited

2. max number of threads [3803] for user [es] is too low, increase to at least [4096]
*               soft    nproc           4096
*               hard    nproc           4096

ERROR: [4] bootstrap checks failed
[1]: 
[2]: 

[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
在   /etc/sysctl.conf文件最后添加一行
vm.max_map_count=262144
生效
[root@localhost elasticsearch]# sysctl -p
vm.max_map_count = 262144
[4]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured

ERROR: Elasticsearch did not exit normally - check the logs at /usr/local/elasticsearch/elasticsearch-7.10.0/logs/my-application.log


2. 
./logstash --path.settings /usr/local/logstash/logstash-7.10.0/ -f /usr/local/logstash/logstash-7.10.0/conf.d/first-demo.conf --config.test_and_exit

[root@localhost bin]# ./logstash --path.settings /usr/local/logstash/logstash-7.10.0/ -f /usr/local/logstash/logstash-7.10.0/conf.d/first-demo.conf --config.test_and_exit
Using JAVA_HOME defined java: /usr/local/java/jdk1.8.0_152
WARNING, using JAVA_HOME while Logstash distribution comes with a bundled JDK

WARNING: Could not find logstash.yml which is typically located in $LS_HOME/config or /etc/logstash. You can specify the path using --path.settings. Continuing using the defaults
Could not find log4j2 configuration at path /usr/local/logstash/logstash-7.10.0/log4j2.properties. Using default config which logs errors to the console
[INFO ] 2020-12-18 17:19:33.895 [main] runner - Starting Logstash {"logstash.version"=>"7.10.0", "jruby.version"=>"jruby 9.2.13.0 (2.5.7) 2020-08-03 9a89c94bcc Java HotSpot(TM) 64-Bit Server VM 25.152-b16 on 1.8.0_152-b16 +indy +jit [linux-x86_64]"}
[INFO ] 2020-12-18 17:19:34.006 [main] writabledirectory - Creating directory {:setting=>"path.queue", :path=>"/usr/local/logstash/logstash-7.10.0/data/queue"}
[INFO ] 2020-12-18 17:19:34.079 [main] writabledirectory - Creating directory {:setting=>"path.dead_letter_queue", :path=>"/usr/local/logstash/logstash-7.10.0/data/dead_letter_queue"}
[WARN ] 2020-12-18 17:19:35.161 [LogStash::Runner] multilocal - Ignoring the 'pipelines.yml' file because modules or command line options are specified
[INFO ] 2020-12-18 17:19:35.251 [LogStash::Runner] configpathloader - No config files found in path {:path=>"/usr/local/logstash/logstash-7.10.0/conf.d/first-demo.conf"}
[ERROR] 2020-12-18 17:19:35.256 [LogStash::Runner] sourceloader - No configuration found in the configured sources.
Configuration OK
[INFO ] 2020-12-18 17:19:35.258 [LogStash::Runner] runner - Using config.test_and_exit mode. Config Validation Result: OK. Exiting Logstash

启动logstash
> nohup ./bin/logstash -f ./bin/conf.d/first-demo.conf > ./bin/nohup.out & 

### 报错
ERROR: Failed to parse YAML file "/usr/local/logstash/logstash-7.10.0/config/logstash.yml". Please confirm if the YAML structure is valid (e.g. look for incorrect usage of whitespace or indentation). Aborting... parser_error=>(<unknown>): expected <block end>, but found '<block mapping start>' while parsing a block mapping at line 249 column 2
[ERROR] 2020-12-18 17:59:08.045 [main] Logstash - java.lang.IllegalStateException: Logstash stopped processing because of an error: (SystemExit) exit

启动zookeeper


启动kafka
> [root@localhost kafka_2.13-2.6.0]# sh bin/kafka-server-start.sh config/server.properties

### 报错
Java HotSpot(TM) 64-Bit Server VM warning: INFO: os::commit_memory(0x00000000c0000000, 1073741824, 0) failed; error='Cannot allocate memory' (errno=12)
#
# There is insufficient memory for the Java Runtime Environment to continue.
# Native memory allocation (mmap) failed to map 1073741824 bytes for committing reserved memory.
# An error report file with more information is saved as:
# /usr/local/kafka/kafka_2.13-2.6.0/hs_err_pid84486.log
错误原因： 
Kafka默认使用-Xmx1G -Xms1G的JVM内存配置，如果机器内存较小，需要调整启动配置。 

解决办法：
打开/config/kafka-server-start.sh，修改 
export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G" 
为适合当前服务器的配置，例如export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"

查看topic列表
> [root@localhost kafka_2.13-2.6.0]# sh bin/kafka-topics.sh -list -zookeeper 192.168.145.10:2181/kafka
```
__consumer_offsets
test
```

查看指定topic属性
> [root@localhost kafka_2.13-2.6.0]# sh bin/kafka-topics.sh -describe -zookeeper 192.168.145.10:2181/kafka -topic test
Topic: test     PartitionCount: 1       ReplicationFactor: 1    Configs: 
        Topic: test     Partition: 0    Leader: 0       Replicas: 0     Isr: 0

创建topic
> sh bin/kafka-topics.sh -topic test -create -partitions 3 -replication-factor 1 -zookeeper 192.168.145.10:2181/kafka
```
-topic 主题名称
-partitions 分区
-replication-factor 副本
```

创建生产者
> [root@localhost kafka_2.13-2.6.0]# sh bin/kafka-console-producer.sh -broker-list 192.168.145.10:9092 -topic test


创建消费者
#重新打开一个ssh连接执行以下命令
> sh bin/kafka-console-consumer.sh --bootstrap-server 192.168.145.10:9092 --topic test --from-beginning

/usr/local/kafka/kafka_2.13-2.6.0/logs/access_log.%Y%m%d

查询group列表
> sh bin/kafka-consumer-groups.sh  --bootstrap-server 192.168.145.10:9092 --list 

查询指定group消费记录，以此判断到底程序是否有消费掉消息从而定位问题
> [es@localhost kafka_2.13-2.6.0]$ sh bin/kafka-consumer-groups.sh --bootstrap-server 192.168.145.10:9092  --describe --group consumer-test 

Consumer group 'consumer-test' has no active members.

GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
consumer-test   test            0          0               4               4               -               -               -

测试
在生成者连接中输入内容. >test
在消费者连接中查看是否接收到消息

kafka相关配置参数
```
broker.id  整数，建议根据ip区分  　
log.dirs  kafka存放消息文件的路径，  默认/tmp/kafka-logs
port  broker用于接收producer消息的端口  　
zookeeper.connnect  zookeeper连接  格式为  ip1:port,ip2:port,ip3:port
message.max.bytes  单条消息的最大长度  　
num.network.threads  broker用于处理网络请求的线程数  如不配置默认为3，server.properties默认是2
num.io.threads  broker用于执行网络请求的IO线程数  如不配置默认为8，server.properties默认是2可适当增大，
queued.max.requests  排队等候IO线程执行的requests  默认为500
host.name  broker的hostname  默认null,建议写主机的ip,不然消费端不配置hosts会有麻烦
num.partitions  topic的默认分区数  默认1
log.retention.hours  消息被删除前保存多少小时  默认1周168小时
auto.create.topics.enable  是否可以程序自动创建Topic  默认true,建议false
default.replication.factor  消息备份数目  默认1不做复制，建议修改
num.replica.fetchers  用于复制leader消息到follower的IO线程数  默认1
```

### 报错
Error while executing topic command : Replication factor: 1 larger than available brokers: 0.
[2020-12-21 09:53:35,630] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 1 larger than available brokers: 0.
 (kafka.admin.TopicCommand$)
[2020-12-21 09:54:58,594] INFO [GroupMetadataManager brokerId=0] Removed 0 expired offsets in 13 milliseconds. (kafka.coordinator.group.GroupMetadataManager)
原因:
  没有在kafka目录下创建zookeeper ，指定myid

[2020-12-21 10:15:19,665] ERROR Fatal error during KafkaServer startup. Prepare to shutdown (kafka.server.KafkaServer)
kafka.common.InconsistentClusterIdException: The Cluster ID 5XQlYCqESuG4_UZhT_19gQ doesn't match stored clusterId Some(77ZSgM4UQsWvE-oeWwHCqw) in meta.properties. The broker is trying to join the wrong cluster. Configured zookeeper.connect may be wrong.
        at kafka.server.KafkaServer.startup(KafkaServer.scala:235)
        at kafka.server.KafkaServerStartable.startup(KafkaServerStartable.scala:44)
        at kafka.Kafka$.main(Kafka.scala:82)
        at kafka.Kafka.main(Kafka.scala)
原因：
	在Kafka的config目录中，打开kafka config属性文件，让server.properties查找具有参数log.dirs =的日志路径目录（log文件），然后转到日志路径目录并在其中找到文件meta.properties。打开文件meta.properties并更新cluster.id =【这个值是error里面有写的】或指定logs目录（本人使用），然后重新启动kafka。

启动kafka-manager
> 安装 yum install java-11-openjdk (https://cloud.tencent.com/developer/article/1651137) 
> cmak -Dconfig.file=../conf/application.conf -java-home /usr/lib/jvm/java-11-openjdk-11.0.9.11-2.el7_9.x86_64
> [root@kafka01 kafka-manager]# nohup bin/kafka-manager >/dev/null 2>&1 &
> https://github.com/yahoo/CMAK/release



启动filebeat
> nohup ./filebeat -e -c filebeat.yml > filebeat.out &

参考
> https://blog.csdn.net/hello_robotdog/article/details/97246664



