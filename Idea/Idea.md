# Idea

### Idea项目发布中Web Application:Exploded和Web Application:Archive的解释
https://blog.csdn.net/ejiao1233/article/details/80444845

### Idea打包war包

### Idea使用@TableField("ID")注解，类调用get、set方法标红的解决办法
> 解决：在setting中，下载lombok插件，安装完成后重启idea。

### Idea编译时报 未结束的字符串字面值
- 原因：编码不一致。
- 解决：Settings ——>> Editor ——>> File Encodings 中，均设置UTF-8，.idea文件夹下检查encodings.xml文件是否均为UTF-8。

### Idea里stash、merge、rebase的使用
* 工作流： 更新操作 ——》 创建本次提交 ——》 推送远程分支
1. 更新操作
```
窗口左侧选择更新类型(Update Type)
Merge：更新时执行合并操作。等价于执行git fetch && git merge或者git pull --no-rebase。
Rebase：更新时执行rebase改写操作。等价于执行git fetch && git rebase或者git pull --rebase。
Branch Default：在.git/config文件中指定不同分支的更新类型。
窗口右侧选择在更新前工作目录(Working Directory)的清理方式：
Using Stash：使用git stash储藏本地修改。
Using Shelve：使用IDEA内置的Shelve功能储藏本地修改。
通常选择Merge和Using Stash即可，单击OK后，IDEA执行步骤如下：
    第1步：使用git stash储藏本地修改
    第2步：执行git fetch && git merge拉取远程分支并合并
    第3步：执行git stash pop恢复储藏
习惯先创建本地提交，然后在执行更新操作，这样会导致Git自动生成一个合并提交，导致提交历史不够简洁。
```
2. 创建本次提交
更新完成后，在IDEA中单击菜单VCS-Commit...创建本次提交。
3. 推送远程分支
然后单击VCS-Git-Push...推送至远程分支。
<blockquote>
	常见问题分析
	最常见的两种意外情况是冲突和文件占用，下面我们分别讨论。
	1. 合并远程分支冲突
	如果在执行更新操作之前，你的本地分支已经创建过提交，并且尚未推送至远程分支，则在第2步执行git merge时很可能会发生冲突。关闭上面的冲突窗口，Version Control工具窗口右下角原本显示分支名称的位置变成了Merging master，表示本地分支master目前处于正在合并状态。单击左侧红框内Resolve按钮可以再次调出处理冲突窗口。基于IDEA的图形界面手动解决冲突后，IDEA会自动将该文件加入暂存区(加入暂存区即表示冲突解决完成)，最后执行一次提交便可以完成冲突处理。
	2. 恢复储藏冲突
	在更新操作的第3步执行git stash pop恢复储藏时，储藏内容可能与刚更新的内容发生冲突。恢复储藏时发生的冲突跟上面的合并冲突稍微有些区别，首先是右下角的分支名称没有Merging字样，另外会在右下角额外弹出一个小窗提示恢复储藏失败，并且告诉你不用担心，所有的修改都在stash列表中，并没有丢失。查看stash列表的方式为单击菜单*VCS-Git-UnStash Changes*。
	选中列表最上面的条目，然后单击*Apply Stash*，之前的修改就会重新回到工作目录。手动解决冲突后执行一次提交就可以了。如果在解决冲突过程中发生了误操作，可以右击*Default Changelist-Revert*...清空当前工作目录内容，重新执行一次Apply Stash，然后重复解决冲突过程。
	3. 文件占用错误
	在执行第2步git merge时，可能会因为文件被占用导致执行失败。例如项目可能引入了一些jar文件，**这些jar文件在本地已经被JVM动态加载了**，如果有其它人更新了该jar文件并且推送到了远程分支，当你更新时便会报错。首先解除文件的占用状态，例如终止本地JVM进程，然后再次重复操作。
	4. 撤销这个提交
	撤销(*reset --soft HEAD*)，然后更新并解决冲突，最后创建一个新的提交。
	5. 代码消失
	调整的代码首选会被IDEA储藏（stash）起来，然后在更新的第2步中仍然会发生冲突，并且发生冲突时，你的修改尚未恢复储藏(unstash)，导致看起来你调整的代码不见了。
	6. Rebase会改写提交历史
	等价于手动执行git fetch && git rebase或者git pull --rebase命令。这样的好处是不会生成一个自动合并提交，保持简洁的提交历史。但是需要注意的是，Rebase之后，你的本地提交会被改写，虽然提交信息一样，但是commit hash已经改变了。
</blockquote>
4. 习惯
* 编码前先更新
* 提交前先更新
* 提交前检查是否有编译错误
* 提交粒度尽可能小，描述尽可能准确
* 修改了公共文件，尽早通知其他成员更新
* 最后一条，也是最重要的，团队分工要明确

### 



