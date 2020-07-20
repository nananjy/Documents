# 【JVM】jmap命令详解----查看JVM内存使用详情

## jmap命令基本概述
jmap命令是一个可以输出所有内存中对象的工具，甚至可以将VM中的heap，以二进制输出成文本。打印出某个java进程（使用pid）内存内的，所有‘对象’的情况（如：产生那些对象，及其数量）。
64位机上使用需要使用如下方式：
>jmap -J-d64 -heap pid

## 参数格式
>jmap [option] <pid> (to connect to running process) 连接到正在运行的进程
><br/>jmap [option] <executable <core> (to connect to a core file)     连接到核心文件
><br/>jmap [option] [server_id@]<remote server IP or hostname> (to connect to remote debug server) 连接到远程调试服务


## 参数说明
>pid:    目标进程的PID，进程编号，可以采用ps -ef | grep java 查看java进程的PID;
><br/>executable:     产生core dump的java可执行程序;
><br/>core:     将被打印信息的core dump文件;
><br/>remote-hostname-or-IP:     远程debug服务的主机名或ip;
><br/>server-id:     唯一id,假如一台主机上多个远程debug服务;

## 基本参数
1. -dump:[live,]format=b,file=<filename> 使用hprof二进制形式,输出jvm的heap内容到文件=.  live子选项是可选的，假如指定live选项,那么只输出活的对象到文件.命令：
>jmap -dump:live,format=b,file=myjmapfile.txt 22431
2. -finalizerinfo 打印正等候回收的对象的信息，命令：
>jmap -finalizerinfo 22431
3. -heap 打印heap的概要信息，GC使用的算法，heap（堆）的配置及JVM堆内存的使用情况.命令：
>jmap -heap 22431
```
[root@CBD-WEB-T1 ~]# ps aux | grep java 
root     22431  0.0  6.1 3712556 241512 ?      Sl    2019 192:16 java -jar mpms-mcsi-send-1.0-SNAPSHOT.jar
[root@CBD-WEB-T1 ~]# jmap -J-d64 -heap 22431 
Attaching to process ID 22431, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.91-b14

using thread-local object allocation.
Parallel GC with 2 thread(s)

Heap Configuration:     #堆配置情况，也就是JVM参数配置的结果[平常说的tomcat配置JVM参数，就是在配置这些]
   MinHeapFreeRatio         = 0                    #最小堆使用比例
   MaxHeapFreeRatio         = 100                  #最大堆可用比例
   MaxHeapSize              = 1004535808 (958.0MB) #最大堆空间大小
   NewSize                  = 20971520 (20.0MB)    #新生代分配大小
   MaxNewSize               = 334495744 (319.0MB)  #最大可新生代分配大小
   OldSize                  = 41943040 (40.0MB)    #老年代大小
   NewRatio                 = 2                    #新生代比例
   SurvivorRatio            = 8                    #新生代与suvivor的比例
   MetaspaceSize            = 21807104 (20.796875MB)  
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:             #堆使用情况【堆内存实际的使用情况】
PS Young Generation
Eden Space:             #伊甸区
   capacity = 20447232 (19.5MB)              #伊甸区容量
   used     = 2232888 (2.1294479370117188MB) #伊甸区使用
   free     = 18214344 (17.37055206298828MB) #伊甸区当前剩余容量
   10.920245830829327% used                  #伊甸区使用情况
From Space:             #survior1区
   capacity = 524288 (0.5MB)        #survior1区容量
   used     = 98304 (0.09375MB)     #surviror1区已使用情况
   free     = 425984 (0.40625MB)    #surviror1区剩余容量
   18.75% used                      #survior1区使用比例
To Space:               #survior2 区
   capacity = 524288 (0.5MB)        #survior2区容量
   used     = 0 (0.0MB)             #survior2区已使用情况
   free     = 524288 (0.5MB)        #survior2区剩余容量
   0.0% used                        # survior2区使用比例
PS Old Generation                   #老年代使用情况
   capacity = 42467328 (40.5MB)     #老年代容量
   used     = 28688656 (27.359634399414062MB)   #老年代已使用容量
   free     = 13778672 (13.140365600585938MB)   #老年代剩余容量
   67.55465283805941% used                      #老年代使用比例

23660 interned Strings occupying 2175432 bytes.
```
4. -histo[:live] 打印每个class的实例数目,内存占用,类全名信息. VM的内部类名字开头会加上前缀”*”. 如果live子参数加上后,只统计活的对象数量.命令：
>jmap -histo:live 22431

5. -permstat 打印classload和jvm heap长久层的信息.包含每个classloader的名字,活泼性,地址,父classloader和加载的class数量.另外,内部String的数量和占用内存数也会打印出来.命令：
>jmap -permstat 22431
6. -F 强迫.在pid没有相应的时候使用-dump或者-histo参数. 在这个模式下,live子参数无效. 
7. -h | -help 打印辅助信息 
8. -J 传递参数给jmap启动的jvm. 
