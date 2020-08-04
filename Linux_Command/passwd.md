# Linux修改密码

- 登录有修改密码权限的账号
- 查看用户登录信息
>[root@FFP-JK-web ~]# id
><br/>uid=0(root) gid=0(root) 组=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel) 环境=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
- 若修改root的密码，直接输入passwd，接着输入两遍密码
```
[root@FFP-JK-web ~]# passwd
更改用户 root 的密码 。
新的 密码：
重新输入新的 密码：
passwd： 所有的身份验证令牌已经成功更新。
```
- 若修改其他用户，如zzz的密码，则输入passwd zzz，接着输入两遍密码
