
## Linux修改用户访问主目录
1. 方法一
> vi /etc/passwd
<br/>找到要修改的用户那几行，修改掉即可。此法很暴力，建议慎用。
2. 方法二
- 查看uid
> [root@SVC-FTP vsftpd]# id svc_user
<br/>`uid=505(svc_user) gid=506(svc_user) 组=506(svc_user)`
- usermod方式修改用户主目录
> usermod -d /home/svc_user -u 505 svc_user
```
-u后面一定要接uid啊，然后是username
附：usermod详细参数
语　　法：usermod [-LU][-c <备注>][-d <登入目录>][-e <有效期限>][- f <缓冲天数>][-g <群组>][-G <群组>][-l <帐号名称>][-s ][-u ] [用户帐号]
补充说明：usermod可用来修改用户帐号的各项设定。
参　　数：
-c<备注> 　修改用户帐号的备注文字。
-d登入目录> 　修改用户登入时的目录。
-e<有效期限> 　修改帐号的有效期限。
-f<缓冲天数> 　修改在密码过期后多少天即关闭该帐号。
-g<群组> 　修改用户所属的群组。
-G<群组> 　修改用户所属的附加群组。
-l<帐号名称> 　修改用户帐号名称。
-L 　锁定用户密码，使密码无效。
-s 　修改用户登入后所使用的shell。
-u 　修改用户ID。
-U 　解除密码锁定
```
- 查看用户列表
> [root@SVC-FTP vsftpd]# less /etc/passwd
<br/>`svc_user:x:505:506::/home/svc_user:/sbin/nologin`
- 

