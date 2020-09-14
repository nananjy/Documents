# rpm+yum包

## 查看已安装的RPM包
- 运行 rpm -qa | grep [package] 

```
[root@test01 ~]# rpm -qa | grep gcc
libgcc-4.4.5-6.el6.x86_64
gcc-c++-4.4.5-6.el6.x86_64
gcc-4.4.5-6.el6.x86_64
```
- 运行 rpm -ql [package] 

```
[root@test01 ~]# rpm -ql mysql
/usr/bin/msql2mysql
/usr/bin/my_print_defaults
/usr/bin/mysql
```
- 运行 yum list installed | grep [package]

```
[root@test01 ~]# yum list installed | grep gcc
gcc.x86_64                          4.4.5-6.el6                        @base    
gcc-c++.x86_64                      4.4.5-6.el6                        @base    
libgcc.x86_64                       4.4.5-6.el6                        @anaconda-RedHatEnterpriseLinux-201105101844.x86_64/6.1
```

