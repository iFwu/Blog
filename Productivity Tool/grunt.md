
Grunt 
----------------
>是一套前端自动化工具，一个基于nodeJs的命令行工具，一般用于：
① 压缩文件
② 合并文件
③ 简单语法检查

Gruntfile.js
----------------
###作用
这个文件尤其关键，他一般干两件事情：
① 读取package信息
② 插件加载、注册任务，运行任务（grunt对外的接口全部写在这里面）

###组成
#####1. 包装函数
这个包装函数没什么东西，意思就是我们所有的代码必须放到这个函数里面
```
module.exports = function (grunt) {
//你的代码
}
```
这个不用知道为什么，直接将代码放入即可

#####2. 项目/任务配置
>读取package信息，调用插件配置一下要执行的任务和实现的功能

我们在Gruntfile一般第一个用到的就是initConfig方法配置依赖信息
```
  // 项目配置
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.file %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%=pkg.file %>.js',
        dest: 'dest/<%= pkg.file %>.min.js'
      }
    }
  });
```

`pkg: grunt.file.readJSON('package.json')`
这里的 grunt.file.readJSON就会将我们的配置文件读出，并且转换为json对象
然后我们在后面的地方就可以采用pkg.XXX的方式访问其中的数据了
这个任务配置其实就是一个方法接口调用。这里只是定义了相关参数，但是并未加载实际函数。

#####3.加载相关插件
>把需要用到的插件加载进来

```
// 加载提供"uglify"任务的插件
grunt.loadNpmTasks('grunt-contrib-uglify');
```

#####4.任务注册
>注册一个task,用来执行你配置的任务。

**任务注册代码**
```
// 默认任务
grunt.registerTask('default', ['uglify']);
```
来注册一个任务。上面代码意思是，你在 default 上面注册了一个 Uglify 任务，default 就是别名，它是默认的 task，当你在项目目录执行 grunt 的时候，它会执行注册到 default 上面的任务。

也就是说，当我们执行 grunt 命令的时候，uglify 的所有代码将会执行。我们也可以注册别的 task，例如：

`grunt.registerTask('compress', ['uglify:build']);`
如果想要执行这个 task，我们就不能只输入 grunt 命令了，我们需要输入`grunt compress`命令来执行这条 task，而这条 task 的任务是 uglify 下面的 build 任务，也就是说，我们只会执行uglify里面 build定义的任务,而不会执行uglify 里面定义的其他任务。

****这里需要注意的是，task 的命名不能与后面的任务配置同名，也就是说这里的 compress 不能命名成 uglify，这样会报错或者产生意外情况****

**任务执行顺序**
`grunt.registerTask('compressjs',['concat','jshint','uglify']);` 
意思就是依次执行 合并、检查、压缩 任务。

###术语扫盲：task & target
>task就是任务，target就是子任务。一个任务可以包含多个子任务。如下所示

```
 grunt.initConfig({
    concat: {   // task
        foo: {  // target
            src: ['a.js', 'b.js'],
            dest: 'ab.js'
        },
        foo2: {
            src: ['c.js', 'd.js'],
            dest: 'cd.js'
        }
    }
  });
```

>每当运行Grunt时, 你可以为其指定一个或多个任务, 这些任务用于告诉Grunt你想要它做什么事情。
当运行此 "任务别名" 时，在 `taskList` 中指定的每个任务都会按照其出现的顺序依次执行。`taskList`参数必须时一个任务数组。

#####任务别名
`grunt.registerTask(taskName, [description, ] taskList)`

下面的任务别名案例中定义了一个 'default' 任务，如果运行Grunt时没有指定任何任务，在控制台运行`grunt`它将自动执行'jshint'、'qunit'、'concat' 和 'uglify' 任务。
**例1**:`grunt.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify']);`

还可以给任务指定参数。在下面的案例中，别名 "dist" 将执行 "concat" 和 "uglify" 两个任务，并且它们都带有一个 "dist" 参数,在控制台运行`grunt dist`,将依次执行'concat'和'dist'任务
**例2**:`grunt.registerTask('dist', ['concat:dist', 'uglify:dist']);`

#####基本任务
`grunt.registerTask(taskName, [description, ] taskFunction)`

在控制台输入`grunt foo`输出，foo no args。
在控制台输入`grunt foo:testing:1234`输出,foo testing 1234。
```
grunt.registerTask('foo', function(arg1, arg2) {
  if (arguments.length === 0) {
    grunt.log.writeln(this.name + " " +"no args");
  } else {
    grunt.log.writeln(this.name + " " + arg1 + " " + arg2);
  }
});
```

#####自定义任务


#####多任务
`grunt.registerMultiTask(taskName, [description, ] taskFunction)`

在控制台运行`grunt log:foo`输出`foo:123`,运行`grunt log:bar`,它会输出`bar: hello world`。然而如果通过`grunt log`运行Grunt, 它会输出`foo: 1,2,3`，然后是`bar: hello world`，最后是`baz: false`

```
grunt.initConfig({
  log: {
    foo: [1, 2, 3],
    bar: 'hello world',
    baz: false
  }
});

grunt.registerMultiTask('log', function() {
  grunt.log.writeln(this.target + ': ' + this.data);
});
```

###Grunt:通配符

+ * 匹配除了 / 之外的任意数量的数字和字符
+ ? 匹配除了 / 之外的单个字符
+ ** 匹配任意数量的字符，包括 /，这样可以包含任意级的路径
+ {} 提供一个以逗号 (,) 分割的或表达式列表
+ ! 放在表达式的开头表示取反

>比如，foo/*.js 将会匹配 foo/ 文件夹下面的所有 .js 扩展名的文件，而 foo/**/*.js 则会匹配在 foo/ 目录下任意级别子目录中的 .js 扩展名的文件。
使用 ! 来不包含特定的文件，需要注意的是 ! 需要是路径的第一个字符。
为了更加简单地通配符，Grunt 允许使用数组来表示通配符。Grunt 将会安装顺序处理，返回的结果是唯一的。

```
// You can specify single files, 简单文件名:
{src: 'foo/this.js', dest: ...}

// Or arrays of files, 使用数组表示多个文件名:
{src: ['foo/this.js', 'foo/that.js', 'foo/the-other.js'], dest: ...}

// Or you can generalize with a glob pattern, 使用通配符:
{src: 'foo/th*.js', dest: ...}

// This single node-glob pattern, 单个通配符:
{src: 'foo/{a,b}*.js', dest: ...}

// Could also be written like this, 通过数组，使用多个通配符:
{src: ['foo/a*.js', 'foo/b*.js'], dest: ...}

// All .js files, in foo/, in alpha order, 所有的 .js 文件，按照字符顺序:
{src: ['foo/*.js'], dest: ...}

// Here, bar.js is first, followed by the remaining files, in alpha order, 第一个是 bar.js， 其它文件按字母顺序 :
{src: ['foo/bar.js', 'foo/*.js'], dest: ...}

// All files except for bar.js, in alpha order, 除了 bar.js 之外的文件，按字母顺序:
{src: ['foo/*.js', '!foo/bar.js'], dest: ...}

// All files in alpha order, but with bar.js at the end, 所有文件按照字母顺序，bar.js 在最后.
{src: ['foo/*.js', '!foo/bar.js', 'foo/bar.js'], dest: ...}

// Templates may be used in filepaths or glob patterns, 可以嵌入表达式:
{src: ['src/<%= basename %>.js'], dest: 'build/<%= basename %>.min.js'}

// But they may also reference file lists defined elsewhere in the config, 引用其它地方定义的文件列表:
{src: ['foo/*.js', '<%= jshint.all.src %>'], dest: ...}
```

项目脚手架
----------
一旦模板被安装到你的~/.grunt-init/目录中,那么就可以通过`grunt-init`命令来使用它们了。

API 
--------
###`grunt option`
>Grunt的option API被用来在多个任务之间共享参数、访问`命令行`中`设置`的`参数`。

```
grunt.initConfig({
  compass: {
    dev: {
      options: {
        /* ... */
        outputStyle: 'expanded'
      },
    },
    staging: {
      options: {
        /* ... */
        outputStyle: 'compressed'
      },
    },
  },
});
var target = grunt.option('target') || 'dev';
grunt.registerTask('deploy', ['compass:' + target]);
```
当你执行`grunt deploy `时，你的样式表将默认为dev目标并且输出易于阅读的CSS格式代码。如果你运行 `grunt deploy --target=staging` ，staging目标会被执行，输出压缩之后的CSS。

`grunt.option`还可以在`task`中使用，如下：
```
grunt.registerTask('upload', 'Upload code to specified target.', function(n) {
  var target = grunt.option('target');
  // do something useful with target here
});
grunt.registerTask('deploy', ['validate', 'upload']);
```

***注意，boolean参数可以仅指定key,而省略value。例如,在命令行执行`grunt deploy --staging`将会使 grunt.option('staging')返回true。***

项目文件传输与协作
----------------
项目开发完成之后，往往需要 push 到 Github 或者上传 FTP 等。或许其他人会接手你的项目继续开发，或者你会换台电脑进行开发。

当小明用 git 上传 Github 的时候，傻了眼，项目里 node_modules 文件夹下面的东西要十几M呢，这比我项目本身还大，上传下载都不方便。

其实这些插件和 grunt 不需要上传，因为有 package.json 这个文件记录了你这个项目中依赖的 grunt 插件，你只需要上传这个文件即可。下载下来之后，只需要在这个项目文件夹下面，输入命令 npm install，NPM 会自动读取 package.json 文件，将 grunt 和有关插件给你下载下来，很方便的。

也不需要在本地上传的时候删除，用 git 的话，可以使用[.gitignore ](https://help.github.com/articles/ignoring-files/)文件来过滤掉这个文件夹，禁止 git 追踪。
```
touch .gitignore //create a .gitignore file.
```

[Grunt 新手一日入门](http://yujiangshui.com/grunt-basic-tutorial/)
[grunt整合版 30分钟学会使用grunt打包前端代码](http://www.cnblogs.com/yexiaochai/p/3603389.html)
[GRUNT](http://www.gruntjs.net/getting-started)



