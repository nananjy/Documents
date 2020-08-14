# Tengine

## 回顾
说起nginx大家都不陌生，以前我们一直用lamp平台（L指Linux，A指Apache，M一般指MySQL，也可以指MariaDB，P一般指PHP），直到后来我们用lnmp平台（L指Linux，N指Nginx，M一般指MySQL，也可以指MariaDB，P一般指PHP），区别就在于apache和nginx区别是nginx并发能力更强，虽然Apache功能强大。

## Tengine定义
Tengine是由淘宝网发起的Web服务器项目。它在Nginx的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如淘宝网，天猫商城等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。

## nginx和tengine的区别
1. tengine是在nginx上面开发的，包含了nginx的性能。
2. tengine更适合大访问量网站的需求，相比nginx更加的稳定，性能更加的强劲。
```
据网络测试：
Tengine相比Nginx默认配置，提升200%的处理能力。
Tengine相比Nginx优化配置，提升60%的处理能力。
参考：https://blog.csdn.net/lianjiangdai/article/details/94907289
```

## Tengine安装
1. 登录服务器配置yum源（以rehl7.4为例）
```
[root@s-app-16-vm yum.repos.d]# cd /etc/yum.repos.d/
[root@s-app-16-vm yum.repos.d]# vi local.repo
注释原本内容，写入如下内容
[local]
name=Red Hat Enterprise Linux - Source
baseurl=ftp://rhel7.4
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/redhat-release
```
2. 安装lrzsz、pcre-devel、openssl openssl-devel组件
```
[root@s-app-16-vm ~]# yum -y install lrzsz
[root@s-app-16-vm ~]# yum -y install pcre-devel
[root@s-app-16-vm ~]# yum -y install openssl openssl-devel
```
3. 解压并安装Tengine安装包（tengine-2.3.1.tar.gz）
```
[root@s-app-16-vm files]# tar -zxvf tengine-2.3.1.tar.gz
[root@s-app-16-vm files]# cd tengine-2.3.1/
[root@s-app-16-vm tengine-2.3.1]# ./configure
[root@s-app-16-vm tengine-2.3.1]# make
[root@s-app-16-vm tengine-2.3.1]# sudo make install
```
4. 安装完毕,切换至安装目录（默认为/usr/local/nginx/）
>[root@s-app-16-vm tengine-2.3.1]# cd /usr/local/nginx/
5. 进入conf配置文件，修改配置内容
```
[root@s-app-16-vm conf]# vi nginx.conf
	在http结构体中（位于server结构体前）新增upstream结构体（动态服务器组，用于声明负载列表，可使用负载策略）
	upstream 10.XX_ngx_s_8180 {
            server 10.XX:8180;
            server 10.XX:8100;
    }
    upstream 10.XX_ngx_m_80 {
            server 10.XX:80;
            server 10.XX:80;
    }
	在server结构体(虚拟主机组)中，修改
	listen       8180;      //监听端口
	server_name  localhost;	//服务名
	在server结构体下的location结构体中新增对应的proxy_pass()
	location / {
		proxy_pass   http://10.XX_ngx_s_8180;            
		root   html;
		index  index.html index.htm;
	}
	对应的 10.XX_ngx_m_80 同样新增一个server结构体并配置对应的监听端口和proxy_pass   
	server{
		listen       80;
        server_name  localhost;
        location / {
            proxy_pass   http://10.XX_ngx_m_80;
            root   html;
            index  index.html index.htm;
        }
        # redirect server error pages to the static page /50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```
6. 启动Tengine
```
[root@s-app-16-vm sbin]# /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
[root@s-app-16-vm sbin]# ps aux|grep nginx
root     51476  0.0  0.0  46040  1192 ?        Ss   16:25   0:00 nginx: master process /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
nobody   51477  0.0  0.0  46444  1956 ?        S    16:25   0:00 nginx: worker process
root     51487  0.0  0.0 112660   944 pts/0    S+   16:25   0:00 grep --color=auto nginx
```
7. 查看日志
>[root@s-app-16-vm sbin]# cd /usr/local/nginx/logs

