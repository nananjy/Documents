# Documents

## Git 操作

1. git clone https://github.com/nananjy/Documents.git
2. cd Documents
3. git init
4. git add .
5. git config --global user.email 'nananjy@foxmail.com'
6. git config --global user.name 'nananjy'
7. git commit -m '提交软件框架'
8. git push
9. git pull --rebase origin master(如果是因为github中的README.md文件不在本地代码目录中， 命令进行代码合并)
10. git clone -b branchA https://github.com/nananjy/Documents.git

## 业内跳转

https://github.com/nananjy/Documents/blob/master/Software_Architecture_Document/1-1_infrastructure/Spring5.md

## Markdown操作

1. 取消自动识别链接
在链接前后添加\`(backtick)扩起来
2. 换行 
方式一，添加\<br/>；方式二，文末添加两个空格字符；方式三，文字前后空一行。
3. 锚点配置
   1. 创建锚点
   ```
   [Oracle安装](#tag1)
   [Oracle表空间](#tag2)
   ```
   2. 跳转到的位置
   ```
   <span id = "tag1">Oracle安装</span>
   <a name = "tag2">Oracle表空间</a>
   ```
  
