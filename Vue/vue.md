# Vue

## 建立运行vue项目
1. 安装node.js
> https://nodejs.org/zh-cn/
<br/>版本 14.15.1

2. 查看node版本
> C:\Users>node -v
<br/>v14.15.1

3. 安装cnpm，由于npm有些国外资源下载不了，安装国内镜像（不建议使用cnpm，据说使用cnpm安装有几率会出现一些诡异的东西，可以在命令后面加-- registry=https://registry.npm.taobao.org）
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated har-validator@5.1.5: this
C:\Users\wzy\AppData\Roaming\npm\cnpm -> C:\Users\wzy\AppData\Roaming\npm\node_modules\cnpm\bin\cnpm
+ cnpm@6.1.1
added 689 packages from 972 contributors in 195.212s
```
4. 安装vue-cli脚手架构建工具
```
npm install vue-cli -g
npm WARN deprecated vue-cli@2.9.6: This package has been deprecated in favour of @vue/cli
npm WARN deprecated coffee-script@1.12.7: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
C:\Users\wzy\AppData\Roaming\npm\vue-list -> C:\Users\wzy\AppData\Roaming\npm\node_modules\vue-cli\bin\vue-list
C:\Users\wzy\AppData\Roaming\npm\vue -> C:\Users\wzy\AppData\Roaming\npm\node_modules\vue-cli\bin\vue
C:\Users\wzy\AppData\Roaming\npm\vue-init -> C:\Users\wzy\AppData\Roaming\npm\node_modules\vue-cli\bin\vue-init
+ vue-cli@2.9.6
added 236 packages from 204 contributors in 95.069s
```
5. 使用vue-cli脚手架构建项目
> vue init webpack first-vue
<br/>初始化一个项目，其中webpack是构建工具，也就是整个项目是基于webpack的。其中first-vue是整个项目文件夹的名称

6. 到项目路径下
> cd D:\vue-view

7. 安装依赖
> npm install
```
	npm ERR! cb() never called!

	npm ERR! This is an error with npm itself. Please report this error at:
	npm ERR!     <https://npm.community>

	npm ERR! A complete log of this run can be found in:
	npm ERR!     C:\Users\wzy\AppData\Roaming\npm-cache\_logs\2020-12-01T09_26_21_243Z-debug.log
```
<blockquote>
	<i>错误解决</i>
	<ul>
		<li>清除你的npm缓存：npm cache clean -f </li>
		npm WARN using --force I sure hope you know what you are doing.
		<li>安装最新版本Node Helper：npm install -g n </li>
		npm ERR! code EBADPLATFORM
		npm ERR! notsup Unsupported platform for n@6.7.1: wanted {"os":"!win32","arch":"any"} (current: {"os":"win32","arch":"x64"})
		npm ERR! notsup Valid OS:    !win32
		npm ERR! notsup Valid Arch:  any
		npm ERR! notsup Actual OS:   win32
		npm ERR! notsup Actual Arch: x64
		npm ERR! A complete log of this run can be found in:
		npm ERR!     C:\Users\wzy\AppData\Roaming\npm-cache\_logs\2020-12-02T01_16_01_348Z-debug.log
		<li>强制安装：npm install -g n --force</li>
		npm WARN using --force I sure hope you know what you are doing.
		C:\Users\wzy\AppData\Roaming\npm\n -> C:\Users\wzy\AppData\Roaming\npm\node_modules\n\bin\n
		+ n@6.7.1
		added 1 package from 4 contributors in 1.018s
		<li>再次运行安装：npm install </li>
		<li>查看npm默认配置：npm config ls -l</li>
		<li>重新安装一次，如果还是不可以的话，在把之前装的都清空，删除文件夹node_modules </li>
		<li>使用cnpm安装：cnpm install</li>
		√ Installed 39 packages
		√ Linked 1 latest versions
		√ Run 0 scripts
		√ All packages installed (2 packages installed from npm registry, used 1m(network 1m), speed 127.08kB/s, json 2(11.58kB), tarball 10.52MB)
	</ul>
</blockquote>
8. 运行项目
    cnpm run dev



