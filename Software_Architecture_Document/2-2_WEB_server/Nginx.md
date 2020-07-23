# Nginx

## 正向代理
科学上网工具就是一个典型的正向代理工具，它会把我们不让访问的服务器的网页请求，代理到一个可以访问该网站的代理服务器上来，一般叫做proxy服务器，再转发给客户,例多客户端一服务端。

## 反向代理
反向代理跟代理正好相反（需要说明的是，现在基本所有的大型网站的页面都是用了反向代理），客户端发送的请求，想要访问server服务器上的内容。发送的内容被发送到代理服务器上，这个代理服务器再把请求发送到自己设置好的内部服务器上，而用户真实想获得的内容就在这些设置好的服务器上，例一客户端多服务端。

## 反向代理的用途和好处
安全性：正向代理的客户端能够在隐藏自身信息的同时访问任意网站，这个给网络安全代理了极大的威胁。因此，我们必须把服务器保护起来，使用反向代理客户端用户只能通过外来网来访问代理服务器，并且用户并不知道自己访问的真实服务器是那一台，可以很好的提供安全保护。
功能性：反向代理的主要用途是为多个服务器提供负债均衡、缓存等功能。负载均衡就是一个网站的内容被部署在若干服务器上，可以把这些机子看成一个集群，那Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上，从而实现服务器压力的平均分配，也叫负载均衡。
参考：https://www.jianshu.com/p/a145a1bc60df

# Nginx 安装

## 安装编译工具及库文件
>[root@iz7uq05oss9ztin71l2axsz files]# yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel

## 首先要安装 PCRE

1. 下载安装包
>pcre-8.43.tar.gz
2. 解压安装包
>[root@iz7uq05oss9ztin71l2axsz files]# cp pcre-8.43.tar.gz /usr/local/
><br/>[root@iz7uq05oss9ztin71l2axsz files]# cd /usr/local/
><br/>[root@iz7uq05oss9ztin71l2axsz local]# tar zxvf pcre-8.43.tar.gz
3. 进入目录
>[root@iz7uq05oss9ztin71l2axsz local]# cd pcre-8.43/
4. 编译安装
>[root@iz7uq05oss9ztin71l2axsz pcre-8.43]# ./configure
><br/>报错 configure: error: Invalid C++ compiler or C++ compiler flags
><br/>原因：没有安装编译工具
><br/>[root@iz7uq05oss9ztin71l2axsz pcre-8.43]# make && make install
5. 查看pcre版本
>[root@iz7uq05oss9ztin71l2axsz pcre-8.43]# pcre-config --version
8.43
6. 如需卸载，到pcre目录下
>make uninstall

## 安装 Nginx

1. 下载 Nginx
>nginx-1.18.0.tar.gz
2. 解压安装包
>[root@iz7uq05oss9ztin71l2axsz local]# cp /opt/files/nginx-1.18.0.tar.gz ./
><br/>[root@iz7uq05oss9ztin71l2axsz local]# tar zxvf nginx-1.18.0.tar.gz
3. 进入安装包目录
>[root@iz7uq05oss9ztin71l2axsz local]# cd nginx-1.18.0/
4. 编译安装
>[root@iz7uq05oss9ztin71l2axsz nginx-1.18.0]# ./configure --prefix=/usr/local/nginx-1.18.0/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/pcre-8.43
><br/>[root@iz7uq05oss9ztin71l2axsz nginx-1.18.0]# make
><br/>[root@iz7uq05oss9ztin71l2axsz nginx-1.18.0]# make install >> log.log
5. 查看nginx版本 
>[root@iz7uq05oss9ztin71l2axsz nginx-1.18.0]# /usr/local/nginx-1.18.0/nginx/sbin/nginx -v
><br/>nginx version: nginx/1.18.0

## Nginx 配置

1. 创建 Nginx 运行使用的用户 www
>[root@bogon conf]# /usr/sbin/groupadd www 
><br/>[root@bogon conf]# /usr/sbin/useradd -g www www
2. 配置nginx.conf ，将/usr/local/webserver/nginx/conf/nginx.conf替换为以下内容
>参考：https://blog.csdn.net/weixin_42167759/article/details/85049546
><br/>[root@bogon conf]#  cat /usr/local/webserver/nginx/conf/nginx.conf
```
user www www;
worker_processes 2; #设置值和CPU核心数一致
error_log /usr/local/webserver/nginx/logs/nginx_error.log crit; #日志位置和日志级别
pid /usr/local/webserver/nginx/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 65535;
events
{
  use epoll;
  worker_connections 65535;
}
http
{
  include mime.types;
  default_type application/octet-stream;
  log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
               '$status $body_bytes_sent "$http_referer" '
               '"$http_user_agent" $http_x_forwarded_for';
  
#charset gb2312;
     
  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 8m;
     
  sendfile on;
  tcp_nopush on;
  keepalive_timeout 60;
  tcp_nodelay on;
  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  gzip on; 
  gzip_min_length 1k;
  gzip_buffers 4 16k;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_types text/plain application/x-javascript text/css application/xml;
  gzip_vary on;
 
  #limit_zone crawler $binary_remote_addr 10m;
 #下面是server虚拟主机的配置
 server
  {
    listen 80;#监听端口
    server_name localhost;#域名
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html;#站点目录
      location ~ .*\.(php|php5)?$
    {
      #fastcgi_pass unix:/tmp/php-cgi.sock;
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_index index.php;
      include fastcgi.conf;
    }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$
    {
      expires 30d;
  # access_log off;
    }
    location ~ .*\.(js|css)?$
    {
      expires 15d;
   # access_log off;
    }
    access_log off;
  }

}
```

   ### https证书部署
   
   - 获取证书
>`Nginx文件夹内获得SSL证书文件 1_www.domain.com_bundle.crt 和私钥文件 2_www.domain.com.key,
1_www.domain.com_bundle.crt 文件包括两段证书代码 “-----BEGIN CERTIFICATE-----”和“-----END CERTIFICATE-----”,
2_www.domain.com.key 文件包括一段私钥代码“-----BEGIN RSA PRIVATE KEY-----”和“-----END RSA PRIVATE KEY-----”。`
   - 证书安装
>`将域名 www.domain.com 的证书文件1_www.domain.com_bundle.crt 、私钥文件2_www.domain.com.key保存到同一个目录，例如/usr/local/nginx/conf目录下。更新Nginx根目录下 conf/nginx.conf 文件如下`
```
server {
        listen 443; #SSL访问端口号为443
        server_name www.domain.com; #填写绑定证书的域名
        ssl on; #启用SSL功能
        ssl_certificate 1_www.domain.com_bundle.crt; #证书文件
        ssl_certificate_key 2_www.domain.com.key; #私钥文件
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#按照这个配置加密套件，写法遵循openssl标准  
        ssl_prefer_server_ciphers on;
        location / {
            root   html; #站点目录
            index  index.html index.htm;
        }
    }
```
>警告nginx: [warn] the "ssl" directive is deprecated, use the "listen ... ssl" directive instead
><br/>ssl不建议作为一个指令使用，而只是listen指令的一个参数。如果使用listen 443 ssl，删除ssl on就行。
   - 使用全站加密，http自动跳转https（可选）
>对于用户不知道网站可以进行https访问的情况下，让服务器自动把http的请求重定向到https。
在服务器这边的话配置的话，可以在页面里加js脚本，也可以在后端程序里写重定向，当然也可以在web服务器来实现跳转。Nginx是支持rewrite的（只要在编译的时候没有去掉pcre）
在http的server里增加rewrite ^(.*) https://$host$1 permanent;
这样就可以实现80进来的请求，重定向为https了。


### 设置自定义Header

- 获取用户的ip地址，比如做异地登陆的判断，或者统计ip访问次数等，通常情况下我们使用request.getRemoteAddr()就可以获取到客户端ip，但是当我们使用了nginx作为反向代理后，使用request.getRemoteAddr()获取到的就一直是nginx服务器的ip的地址；
- 获取客户端浏览器url。
```
proxy_set_header Host $host; 
获取客户端访问的头部，它的值在请求包含"Host"请求头时为"Host"字段的值，在请求未携带"Host"请求头时为虚拟主机的主域名。
```
[Nginx反向代理配置与测试](2-2_WEB_server/Nginx/Nginx反向代理配置与测试.md)

3. 检查配置文件nginx.conf的正确性命令
>[root@bogon conf]# /usr/local/webserver/nginx/sbin/nginx -t

## 启动 Nginx

1. Nginx 启动命令
>[root@bogon conf]# /usr/local/webserver/nginx/sbin/nginx

## 访问站点

## Nginx 其他命令
1. 重新载入配置文件
>/usr/local/webserver/nginx/sbin/nginx -s reload
2. 重启 Nginx
>/usr/local/webserver/nginx/sbin/nginx -s reopen
3. 停止 Nginx
>/usr/local/webserver/nginx/sbin/nginx -s stop


