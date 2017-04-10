# 目录
## node.js安装
## npm 的使用
## node.js 升级
## nvm 的安装及使用
## 查看远程node.js版本信息
## 查看本地所有node.js版本信息
## 简单运行node.js 的DEMO


----------------------------------------------------
----------------------------------------------------
----------------------------------------------------


# Node.js 安装
## ubuntu 下的安装
### 安装node.js的方法
### package.json文件的配置

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
#### `package.json`文件可以手工编写,也可以使用`npm init ` 命令自动生成
```
$ npm init            # 需要自己填写package.json文件
$ npm init --yes      # 跳过package.json文件的填写 直接生成
```
这个命令采用互动方式，要求用户回答一些问题，然后在当前目录生成一个基本的package.json文件。  
所有问题之中，只有项目名称（name）和项目版本（version）是必填的，其他都是选填的。  

有了package.json文件，直接使用`npm install`命令，就会在当前目录中安装所需要的模块。  
package.json文件会指定这个项目要用的第三方npm包
```
$ npm install
```
如果一个模块不在package.json文件之中，可以单独安装这个模块，并使用相应的参数，将其写入package.json文件之中。
```
$ npm install express --save
$ npm install express --save-dev
```
上面代码表示单独安装`express`模块，`--save`参数表示将该模块写入`dependencies`属性，  
`--save-dev`表示将该模块写入`devDependencies`属性。  
(`dependencies`属性   项目发布  
`devDependencies`属性   项目开发研究时)


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
### scripts字段
`scripts`指定了运行脚本命令的`npm`命令行缩写，比如`start`指定了运行`npm run start`时，所要执行的命令。  
下面的设置指定了`npm run preinstall`、`npm run postinstall`、`npm run start`、`npm run test`时，所要执行的命令。
```
"scripts": {
    "preinstall": "echo here it comes!",
    "postinstall": "echo there it goes!",
    "start": "node index.js",
    "test": "tap test/*.js"
}
```
### dependencies字段, devDependencies字段
`dependencies`字段指定了项目运行所依赖的模块,`devDependencies`指定项目开发所需要的模块.  
它们都指向一个对象.该对象的各个成员,分别由模块名和对应的版本要求组成,表示依赖的模块及其版本范围
```json
{
  "devDependencies": {
    "browserify": "~13.0.0",
    "karma-browserify": "~5.0.1"
  }
}
```
对应的版本可以加上各种限定,只要有以下几种:  
> 指定版本：比如1.2.2，遵循“大版本.次要版本.小版本”的格式规定，安装时只安装指定版本。
>> * 波浪号（tilde）+指定版本：比如~1.2.2，表示安装1.2.x的最新版本（不低于1.2.2），但是不安装1.3.x，也就是说安装时不改变大版本号和次要版本号。
>>  * 插入号（caret）+指定版本：比如ˆ1.2.2，表示安装1.x.x的最新版本（不低于1.2.2），但是不安装2.x.x，也就是说安装时不改变大版本号。  
     需要注意的是:如果大版本号为0，则插入号的行为与波浪号相同，这是因为此时处于开发阶段，即使是次要版本号变动，也可能带来程序的不兼容。
>>  *  latest：安装最新版本。

# Node.js 升级
node有一个模块叫 `n`，是专门用来管理 `node.js `的版本安装并将当前的`nodejs`版本升级为`stable`最新版本
```
$ npm install -g n        //安装nodejs
$ sudo n stable           //升级nodejs到stable版本
```
显示当前nodejs版本信息
```
$ node -v
```
## Nvm 使用
总的来说，`nvm`有点类似于 `Python` 的 `virtualenv` 或者 `Ruby` 的 `rvm`，  
每个`node`版本的模块都会被安装在各自版本的沙箱里面（因此切换版本后模块需重新安装），  
因此考虑到需要时常对`node`版本进行切换测试兼容性和一些模块对`node`版本的限制，我选择了使用`nvm`作为管理工具，  
下面就来说说`nvm`的安装和使用过程。
### nvm 安装
```
 $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash
```
完成后`nvm`就被安装在了 `~/.nvm `下啦，接下来就需要配一下环境变量了，如果你也使用了` zsh `的话，  
就需要在` ~/.zshrc `这个配置文件中配置，否则就找找看` ~/.bash_profile` 或者 `~/.profile` 吧。  

在 Ubuntu 下 直接 操作 vim ~/.bashrc ,在最后一行上加上:
```js
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```
在执行下面一步操作,作用是每次新打开一个bash,nvm都会被自动添加到环境变量中了.  
```
$ . ./bashrc
```
配置完成后,输入 `source~/.zshrc`重新启动一下配置

输入`nvm`可以看到如下信息:
```json
➜  ~  nvm

Node Version Manager

Note: <version> refers to any version-like string nvm understands. This includes:
  - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
  - default (built-in) aliases: node, stable, unstable, iojs, system
  - custom aliases you define with `nvm alias foo`

Usage:
  nvm help                                  Show this message
  nvm --version                             Print out the latest released version of nvm
  nvm install [-s] <version>                Download and install a <version>, [-s] from source. Uses .nvmrc if available
    --reinstall-packages-from=<version>     When installing, reinstall packages installed in <node|iojs|node version number>
  nvm uninstall <version>                   Uninstall a version
  nvm use [--silent] <version>              Modify PATH to use <version>. Uses .nvmrc if available
  nvm exec [--silent] <version> [<command>] Run <command> on <version>. Uses .nvmrc if available
  nvm run [--silent] <version> [<args>]     Run `node` on <version> with <args> as arguments. Uses .nvmrc if available
  nvm current                               Display currently activated version
  nvm ls                                    List installed versions
  nvm ls <version>                          List versions matching a given description
  nvm ls-remote                             List remote versions available for install
  nvm version <version>                     Resolve the given description to a single local version
  nvm version-remote <version>              Resolve the given description to a single remote version
  nvm deactivate                            Undo effects of `nvm` on current shell
  nvm alias [<pattern>]                     Show all aliases beginning with <pattern>
  nvm alias <name> <version>                Set an alias named <name> pointing to <version>
  nvm unalias <name>                        Deletes the alias named <name>
  nvm reinstall-packages <version>          Reinstall global `npm` packages contained in <version> to current version
  nvm unload                                Unload `nvm` from shell
  nvm which [<version>]                     Display path to installed node version. Uses .nvmrc if available

Example:
  nvm install v0.10.32                  Install a specific version number
  nvm use 0.10                          Use the latest available 0.10.x release
  nvm run 0.10.32 app.js                Run app.js using node v0.10.32
  nvm exec 0.10.32 node app.js          Run `node app.js` with the PATH pointing to node v0.10.32
  nvm alias default 0.10.32             Set default node version on a shell

Note:
  to remove, delete, or uninstall nvm - just remove the `$NVM_DIR` folder (usually `~/.nvm`)


```

# 查看远程node.js版本信息
我们可以查看现在官方网站有多少个最新版本
```
$ nvm ls-remote
```
# 查看本地所有node.js版本信息
可以查看我们现在使用的是那个版本
```
$ nvm ls
```
# 简单运行node.js 的DEMO
我们现在需要写一个简单的例子, 测试一下是否安装成功. 现在打开编辑工具, 编写下在面的代码块. 创建一个叫`demo.js`的文件.
```js
var http = require('http');
var serv = http.createServer(function(req, res){
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end('<h1>Hello Welcome to Node.js!</h1>');
});
serv.listen(3000);
```
需要运行node.js命令：
```
$ node demo.js 
```
打开浏览器访问`http//127.0.0.1:3000`. 可以看到我们刚才写的`Hello Welcome to Node.js!`句话.  

这就是用最Node.js实现最简单的HTTP服务器就这样延生了. 这个程序调用了Node.js提供的http模块.对所有http请求答复同样的内容监听3000端口.

