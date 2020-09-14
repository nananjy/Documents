# Eclipse

## 编写一个简单的main类启动时报无法加载主类的处理方法
1. 点开Problems栏，查看问题列表。
<br/>Problems可以通过工具栏上windows--show打开对话框，如果是多个项目，可以将其他项目暂时关闭。
2. 清理所有的错误信息。
<br/>对错误先进性清理掉，因为可能有其他项目工程影响，所以清理所有的错误信息，在错误列表上右键Delete。
3. 重新刷新项目，更新错误信息，方便对问题进行定位。
<br/>在项目名称上右键--Refresh。
4. 清理项目里面的class文件进行重新编译。
<br/>鼠标点击项目名称上，然后点击工具栏上Project——》Clean。
5. 错误一：Java Compiler level【java编译版本】。
<br/>项目上右键Properties——》Java Compile——》选择java版本。
6. 错误二：缺少依赖的jar包。
<br/>项目右键java build path——》add exteral jars添加依赖包。
