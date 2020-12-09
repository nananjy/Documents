# HIVE

## Hive 分区管理
1. 新建分区
> alter table db.wifi add if not exists partition (dt = '2020-11-01') 
2. 删除分区
> alter table db.wifi drop partition (dt = '2020-11-01') 
3. 查询所有分区
> show partitions db.wifi 
4. 查询分区所在位置
> hdfs dfs -ls /user/hive/warehouse/db.db/wifi 
<br/>或者
<br/> hadoop fs -ls /user/hive/warehouse/db.db/wifi 
5. 删除分区
> hdfs dfs -rm -r -f /user/hive/warehouse/db.db/wifi 

