>NPM（node package manager）
主要功能:安装、卸载、更新、查看、搜索、发布

1.npm的安装、卸载、升级、配置
2.npm的使用：package的安装、卸载、升级、查看、搜索、发布
   
   + npm包的安装模式,本地 vs 全局
   + package版本：常见版本声明形式

3.package.json：包描述信息

npm 
-------
###配置
`npm set`

```
npm set init-author-name 'Your name'
npm set init-author-email 'Your email'
npm set init-author-url 'http://yourdomain.com'
npm set init-license 'MIT'
```
上面命令等于为`npm init`设置了默认值，以后执行`npm init`的时候，package.json的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的`~/.npmrc`文件，使得用户不用每个项目都输入。
如果某个项目有不同的设置，可以针对该项目运行`npm config`。
`1.npm set save-exact true`
上面命令设置加入模块时，package.json将记录模块的确切版本，而不是一个可选的版本范围。

`2.npm config get prefix`

`3.npm config set prefix /usr/local`

npm使用
-------
###安装模块

`npm install grunt-cli`
安装之前，npm install会先检查，node_modules目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。
如果你希望，一个模块不管是否安装过，npm 都要强制重新安装，可以使用-f或--force参数。

+ 本地安装: package会被下载到当前所在目录，也只能在当前目录下使用。安装结束后，当前目录下回多出一个node_modules目录，grunt-cli就安装在里面。
`npm install -g grunt-cli`
+ 全局安装:package会被下载到到特定的系统目录下，安装的package能够在所有目录下使用。现在变成了/usr/local/lib/node_modules/grunt-cli，/usr/local/lib/node_modules/也就是之前所说的全局安装目录啦。

1.安装当前目录package.json文件中配置的devDependencies模块
`npm install`

2.安装本地的模块文件
`npm install ./package.tgz`
3.安装指定URL的模块
`npm install https://github.com/indexzero/forever/tarball/v0.5.6`
4.安装本地文件系统中指定的目录包含的模块
`npm install <folder>`
5.安装并更新package.json中的版本配置
`npm install <name> [–save|–save-dev|–save-optional]`

+ 添加–save 参数安装的模块的名字及其版本信息会出现在package.json的dependencies选项中
+ 添加–save-dev 参数安装的模块的名字及其版本信息会出现在package.json的devDependencies选项中
+ 添加–save-optional 参数安装的模块的名字及其版本信息会出现在package.json的optionalDependencies选项中

6.安装模块的指定版本
`npm install <name>@<version>`
Example:
`npm install underscore@1.5.2`
7.安装模块指定版本号范围内的某一个版本
`npm install <name>@<version range>`
Example:
`npm install async@”>=0.2.0 <0.2.9″`

`–force`强制拉取远程资源，即使本地已经安装这个模块
Example:
`npm install underscore –force`

8.`-g或–global`全局安装模块，如果没有这个参数，会安装在当前目录的node_modules子目录下
Example:
`npm install -g express`

如果你希望，所有模块都要强制重新安装，那就删除node_modules目录，重新执行npm install。
```
$ rm -rf node_modules
$ npm install
```

###更新模块
`npm update [-g] [<name> [<name> … ]`
更新指定name列表中的模块。-g参数更新全局安装的模块。
如果没有指定name，且不是在某个模块内，会更新当前目录依赖的所有包都会被更新（包括全局和模块内）；如果当前目录在某个模块目录内，会更新该模块依赖的模块，所以不指定name直接运行npm update时，最好在某个模块内运行，以免更新到其他不想更新的模块。

###卸载模块
`npm uninstall package`

###查看
1. 查看安装了那些包
`npm ls --depth=0`
2. 查看特定包具体信息
`npm ls grunt-cli`
`npm info grunt-cli`

###搜索
`npm search grunt-cli`

###发布

package.json
---------------

###package.json说明
npm命令运行时会读取当前目录的 package.json 文件和解释这个文件，这个文件基于 [Packages/1.1](http://wiki.commonjs.org/wiki/Packages/1.1)规范。在这个文件里你可以定义你的应用名称( name )、应用描述( description )、关键字( keywords )、版本号( version )、应用的配置项( config )、主页( homepage )、作者( author )、资源仓库地址( repository )、bug的提交地址( bugs )，授权方式( licenses )、目录( directories )、应用入口文件( main )、命令行文件( bin )、应用依赖模块( dependencies )、开发环境依赖模块( devDependencies )、运行引擎( engines )和脚本( scripts )等。

对于开发者而言，开发和发布模块都依赖于他对这个文件 package.json 所包含的意义的正确理解。我们下面用一个本文共用的例子来说明：

```
{
    "name": "test",
    "version": "0.1.0",
    "description": "A testing package",
    "author": "A messed author <messed@example.com>",
    "dependencies": {
        "express": "1.x.x",
        "ejs": "0.4.2",
        "redis": ">= 0.6.7"
    },
    "devDependencies": {
        "vows": "0.5.x"
    },
    "main": "index",
    "bin": {
        "test": "./bin/test.js"
    },
    "scripts": {
        "start": "node server.js",
        "test": "vows test/*.js",
        "preinstall": "./configure",
        "install": "make && make install"
    },
    "engines": {
        "node": "0.4.x"
    }
}
```

这个例子里我们定义了应用的入口文件( main )为 index ，当其他应用引用了我们的模块 require('test') 时，这个 main 的值 index.js 文件被调用。脚本( scripts )使用hash 表定义了几个不同的命令。script.start 里的定义的 node server.js 会在 npm start 时被调用，同样的 npm test 调用时对应的 scripts.test 里定义的命令被调用。在有些 native 模块需要编译的话，我们可以定义预编译和编译的命令。

本例中还定义了应用依赖模块( dependencies )和开发环境依赖模块( devDependencies )。应用依赖模块会在安装时安装到当前模块的 node_modules 目录下。开发环境依赖模块主要时在开发环境中用到的依赖模块，用命令 npm 的命令 install 或 link 加上参数 —dev 安装到当前模块的 node_modules 目录下。

1. name: package的名字（由于他会成为url的一部分，所以 non-url-safe 的字母不会通过，也不允许出现"."、"_"），最好先在[](http://registry.npmjs.org/上搜下你取的名字是否已经存在)
2. version: package的版本，当package发生变化时，version也应该跟着一起变化，同时，你声明的版本需要通过semver的校验（semver可自行谷歌）
3. dependencies: package的应用依赖模块，即别人要使用这个package，至少需要安装哪些东东。应用依赖模块会安装到当前模块的node_modules目录下。
4. devDependencies：package的开发依赖模块，插件发布的时候自动删除不相关代码。用个文件记录一下当前项目中安装或者需要的插件，即别人要在这个package上进行开发，可以一键安装项目所需插件。
其他：参见官网

###版本号
大家也注意到 package.json 里的版本号有些是 >= 0.6.7 有些是 1.x.x，这有什么区别？npm 使用于[语义化的版本识别](http://semver.org/lang/zh-CN/)来进行版本管理。并不是所有的模块都会提供向后兼容性，有时候某些模块因为某些原因导致不向后兼容。所以我们需要定义一些规则来保证模块能够在某些特定的版本中可用，并且保证能用最新的版本，因为那些版本总是修改了一些 bug 或提升了性能等。我们来看一下版本定义的字段：
```
例子:0.4.2

+ 大版本(0)
+ 小版本(4)
+ 补丁版本(2)
```

一个软件发布的时候，默认就是 1.0.0 版。如果以后发布补丁，就增加最后一位数字，比如1.0.1；如果增加新功能，且不影响原有的功能，就增加中间的数字（即小版本号），比如1.1.0；如果引入的变化，破坏了向后兼容性，就增加第一位数字（即大版本号），比如2.0.0。

在上面 package.json 的定义里我们确信模块在所有的 Nodejs 0.4及以上和0.5以下版本里都能运行。依赖模块 redis 在所有大于或等于0.6.7的版本上都能运行，依赖模块 ejs 只能确保运行在0.4.2版本里，依赖模块 express 确保能够兼容大于或等于1.0.0并且小于2.0.0。

###生成
`npm init`
用来初始化生成一个新的package.json文件。它会向用户提问一系列问题，如果你觉得不用修改默认配置，一路回车就可以了。
如果使用了-f（代表force）、-y（代表yes），则跳过提问阶段，直接生成一个新的package.json文件。

[npm官方文档](https://docs.npmjs.com/)
[npm模块管理器](http://javascript.ruanyifeng.com/nodejs/npm.html#toc22)
[如何使用NPM来管理你的Node.js依赖](http://www.infoq.com/cn/articles/msh-using-npm-manage-node.js-dependence)




