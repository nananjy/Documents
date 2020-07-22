# OpenSSL
OpenSSL是一个开放源代码的软件库包，应用程序可以使用这个包来进行安全通信，避免窃听，同时确认另一端连接者的身份。这个包广泛被应用在互联网的网页服务器上。

## Linux系统下生成单向认证证书 https证书
参考：https://blog.csdn.net/duanbokan/article/details/50847612

1. 生成秘钥key
```
[root@svc-app-t28 nginx-1.18.0]# openssl genrsa -des3 -out server.key 2048
Generating RSA private key, 2048 bit long modulus
....................................................................+++
.....................................................................+++
e is 65537 (0x10001)
Enter pass phrase for server.key:~server~
Verifying - Enter pass phrase for server.key:
```
2. 省略输入密码（通过openssl提供的命令或API，要求输入密码）
```
[root@svc-app-t28 nginx-1.18.0]# openssl rsa -in server.key -out server.key
Enter pass phrase for server.key:
writing RSA key
```
3. 创建服务器证书的申请文件server.csr
```
[root@svc-app-t28 nginx-1.18.0]# openssl req -new -key server.key -out server.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:~CN~
State or Province Name (full name) []:.
Locality Name (eg, city) [Default City]:.
Organization Name (eg, company) [Default Company Ltd]:.
Organizational Unit Name (eg, section) []:.
Common Name (eg, your name or your server's hostname) []:.
Email Address []:.

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:.
An optional company name []:.
```
4. 创建CA证书，给自己的证书签名
```
[root@svc-app-t28 nginx-1.18.0]# openssl req -new -x509 -key server.key -out ca.crt -days 3650
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:~CN~
State or Province Name (full name) []:.
Locality Name (eg, city) [Default City]:.
Organization Name (eg, company) [Default Company Ltd]:.
Organizational Unit Name (eg, section) []:.
Common Name (eg, your name or your server's hostname) []:.
Email Address []:.
```
5. 创建自当前日期起有效期为期十年的服务器证书server.crt
```
[root@svc-app-t28 nginx-1.18.0]# openssl x509 -req -days 3650 -in server.csr -CA ca.crt -CAkey server.key -CAcreateserial -out server.crt
Signature ok
subject=/C=CN
Getting CA Private Key
```
6. 生成完成，转向配置nginx。







