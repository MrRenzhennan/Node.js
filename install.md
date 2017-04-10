
# Node.js 安装
## ubuntu 下的安装
### 安装node.js的方法
```
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```
## npm 的使用
`NPM`是随同`NodeJS`一起安装的安装管理工具,能解决`NodeJS`代码部署上的很多问题,  
常见的使用场景有以下几种:  
```
1. 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
2. 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
3. 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。
```
由于新版的`node.js`已经集成了`npm`,所以在安装`node.js`的同时,连带上npm也一并安装好,  
可以通过`"npm -v"`来检测npm是否安装成功.
```
$npm -v
3.10.9
```
## npm服务器 包 的全局安装与本地安装
### npm 的包安装分为本地安装(local) 和 全局安装(global) 两种
```
$ npm install express     # 本地安装
$ npm install express -g  # 全局安装
```
### 本地安装
1. 将安装包放在`./node_modules` 下`(运行 npm 命令时所在的目录)`,如果没有`node_modules`目录,  
   会在当前执行 `npm` 命令的目录下生成`node_modules`目录.
2. 可以通过 `require()`来引入本地安装的包.
### 全局安装
1. 将安装包放在`/usr/local`下或者你的`node`的安装目录.
2. 可以在命令行里使用.
### 查看全局安装的模块
使用下面命令查看所有全局安装的模块:  
```
$ npm ls -g
```
### 强制重新安装
如果你忘记一个模块是否安装过,`npm`都要强制重新安装,可以使用`-f`或`--force`参数.
```
$ npm install <packageName> --force
```
### 更新已经安装的模块
如果想更新已安装模块，就要用到npm update命令。  
它会先到远程仓库查询最新版本，然后查询本地版本。如果本地版本不存在，或者远程版本较新，就会安装.
```
$ npm update <packageName>
```
## 使用 package.json
`package.json`文件描述了一个`NPM`包的所有相关信息，包括作者、简介、包依赖、构建等信息。  
格式必须是严格的`JSON格式`。package.json 位于模块的目录下，用于定义包的属性。  
每个项目的根目录下面，一般都有一个`package.json`文件，定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。  
`npm install命令`根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。  
* package.json文件内部就是一个JSON对象  
我们现在看一个完整的package.json文件
```json
{
	"name": "Hello World",
	"version": "0.0.1",
	"author": "张三",
	"description": "第一个node.js程序",
	"keywords":["node.js","javascript"],
	"repository": {
		"type": "git",
		"url": "https://path/to/url"
	},
	"license":"MIT",
	"engines": {"node": "0.10.x"},
	"bugs":{"url":"http://path/to/bug","email":"bug@example.com"},
	"contributors":[{"name":"李四","email":"lisi@example.com"}],
	"scripts": {
		"start": "node index.js"
	},
	"dependencies": {
		"express": "latest",
		"mongoose": "~3.8.3",
		"handlebars-runtime": "~1.0.12",
		"express3-handlebars": "~0.5.0",
		"MD5": "~1.2.0"
	},
	"devDependencies": {
		"bower": "~1.2.8",
		"grunt": "~0.4.1",
		"grunt-contrib-concat": "~0.3.0",
		"grunt-contrib-jshint": "~0.7.2",
		"grunt-contrib-uglify": "~0.2.7",
		"grunt-contrib-clean": "~0.5.0",
		"browserify": "2.36.1",
		"grunt-browserify": "~1.3.0",
	}
}
```
## scripts字段
`scripts`指定了运行脚本命令的`npm`命令行缩写，比如`start`指定了运行`npm run start`时，所要执行的命令。  

