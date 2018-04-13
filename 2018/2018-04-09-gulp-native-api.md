# gulp-api
> 简单说来gulp原本不支持任何功能，只提供最基础的Api（4个）。<br /> 
他们分别是gulp.src, gulp.dest, gulp.task, gulp.watch。<br />
 因此，想要简单的使用gulp很容易，但是想要将gulp使用到得心应手的地步并不是一件简单的事情。 <br />
 而其中最关键的地方在于，对nodejs文件路径匹配模式globs的理解。

- gulp.task创建一个任务
- gulp.src表示创建的任务是针对文件目录中的那些位置
- gulp.dest表示目标文件被处理完毕之后会在哪个位置重新生成
- gulp.watch表示监听那些位置文件的变化

> 总结：除了task，其他三个api都与文件路径密切相关，<br />
而gulp.src与gulp.watch更是直接以golbs为第一个参数， <br />
gulp的各个插件需要知道自己处理的是那些位置的文件，而golbs则负责指定需要被处理文件的位置<br />
每个人对前端的理解有所差异，造成了前端的项目结构也是各不一样，<br />
因此golbs的配置也就显得更加重要 下面就先了解一下golbs的匹配规则。

## Glob-Primer

[node-glob语法](https://github.com/isaacs/node-glob#glob-primer);

![glob-table-parttern](https://github.com/kkcode24/blog/blob/master/2018/images/2018-04-13-glob.png)

**匹配文件中0个或者多个字符，但是不会匹配路径中的分隔符，除非路径分隔符出现在末尾**

```javascript
// 匹配src目录下所有的js文件
src/*.js

// 匹配src目录下所有的文件
src/*.*

// 只要层级相同，可以匹配任意目录下的任意js文件 比如src/a/b.js
src/*/*.js

```

**匹配文件路径中的0个或者多个层级的目录，需要单独出现，如果出现在末尾，也可匹配文件**

```javascript
// 递归获取src目录下的所有文件及目录及目录下的所有文件
src/**/*.*

如可以匹配到
src/index.html
src/js
src/js/a.js
src/images
src/images/c.png
```

**? 匹配一个字符，不会匹配路径分隔符**
```javascript
// 能匹配文件名只有一个字符的js文件，如a.js, b.js ,但不能匹配文件名为2个字符及其以上的js的文件
src/js/?.js

如可以匹配到：
src/js/a.js
src/js/b.js
不可以匹配到：
src/js/aa.js
src/js/bb.js
```

**[...] 由多个规则组成的数组，可以匹配数组中符合任意一个子项的文件，当子项中第一个字符为!或者^时，表示不匹配该规则**
```javascript
// 匹配src目录下的a0.js, a1.js, a2.js, a3.js
'src/a[0-3].js'

// 除开node_modules目录之外，匹配项目根目录下的所有html文件
['./**/*.html', '!node_modules/**']
```

**{...} 展开模式，根据里面的内容展开为多个规则，能匹配所有展开之后的规则**
```javascript
// 除开build,simple,images,node_modules目录，匹配根目录下所有的html与php文件
['./**/*.{html, php}', '!{build, simple, images, node_modules}/**']
```

**!(pattern|pattern|pattern) 每一个规则用pattern表示，这里指排除符合这几个模式的所有文件**
```javascript
// 匹配排除文件名为一个字符的js文件，以及排除jquery.js之后的所有js文件
'src/js/!(?|jquery).js'

// 排除build与node_modules目录，并排除其他目录下以下划线_开头的html与php文件，匹配其余的html与php文件
// 这种比较复杂的规则在实际开发中会常常用到，需要加深了解
['src/**/!(_)*.{html, php}', '!{build, node_modules}/**']
```

**?(pattern|pattern|pattern) 匹配括号中给定的任一模式0次或者1次**
```javascript
// 匹配src/js目录下的a.js, a2.js, b.js
// 不能组合
// 匹配0次或者1次
'scr/js/?(a|a2|b).js'
```

**@(pattern|pattern|pattern) 匹配多个模式中的任一个**
```javascript
// 匹配src/js目录下的a.js, b.js, c.js
// 不能组合
// 匹配一次，不能为空，注意与?的区别
'./src/js/@(a|b|c).js'
```

**+(pattern|pattern|pattern) 匹配括号中给定任一模式1次或者多次，这几个模式可以组合在一起匹配**
```javascript
// 可以匹配src/js目录下的a.js, a2.js, b.js
// 也可以匹配他们的组合 ab.js, aa2.js, a2b.js等
// 至少匹配一次，为空不匹配
'./src/js/+(a|a2|b).js'
```

***(pattern|pattern|pattern) 匹配括号中给定任一模式0次或者多次，这几个模式可以组合在一起匹配**
```javascript
// 可以匹配src/js目录下的a.js, b.js, c.js
// 也可以匹配他们的组合 ab.js, bc.js, ac.js
// 匹配0次或者多次
'./src/js/*(a|b|c).js'
```

> 几乎能用到的规则就都在上面了，在实际使用的时候，我们需要根据自己的实际情况来使用. <br />
只要掌握了globs模式，距离精通gulp也不太远了。



## gulp.src(globs[, options])
> 在介绍这个API之前我们首先来说一下Grunt.js和Gulp.js工作方式的一个区别。<br>
Grunt主要是以文件为媒介来运行它的工作流的，比如在Grunt中执行完一项任务后，会把结果写入到一个临时文件中，<br/>
然后可以在这个临时文件内容的基础上执行其它任务，执行完成后又把结果写入到临时文件中，<br>
然后又以这个为基础继续执行其它任务...就这样反复下去。<br>
而在Gulp中，使用的是Nodejs中的stream(流)，首先获取到需要的stream，<br>
然后可以通过stream的pipe()方法把流导入到你想要的地方，<br>
比如Gulp的插件中，经过插件处理后的流又可以继续导入到其他插件中，当然也可以把流写入到文件中<br>
。所以Gulp是以stream为媒介的，它不需要频繁的生成临时文件，这也是Gulp的速度比Grunt快的一个原因。<br>
再回到正题上来，gulp.src()方法正是用来获取流的，但要注意这个流里的内容不是原始的文件流，而是一个虚拟文件对象流(Vinyl files)，<br>
这个虚拟文件对象中存储着原始文件的路径、文件名、内容等信息，这个我们暂时不用去深入理解。

> gulp.src指定gulp任务的目标文件位置 它的参数中，第一个参数globs为必选。 <br />
中括号表示可选参数，options为一个配置json对象 globs用于指定目标文件的位置，<br />
在上面已经做过介绍，这里对option做一个简单的介绍即可


```javascript
gulp.src('./**/!(_).{php, html}', {
    buffer: true,
    read: true,
    base: './'
})
```

**options.buffer**
> 类型: Boolean 默认：true 如果该项被设置为false，那么将会以stream方式返回file.contents，而不是文件buffer的形式。 这在处理一些大文件的时候非常有用。 另外需要注意点是，gulp插件可能不支持stream

**options.read**
> 类型: Boolean 默认: true 如果被设置为false，那么file.contents会返回空值(null) 也就是并不会去读取文件

**options.base**
> 类型: String 默认值: 第一个参数目录中，glob匹配模式之前的路径 理解base至关重要，它将影响到文件处理完毕后，新文件生成的目录 我们需要结合gulp.dest来理解它。

比如在一个路径为static/scripts/libs的目录中，有一个js组件叫做dialog.js
```javascript
// 匹配static/scripts/libs/dialog.js，并将base解析成 static/scripts
gulp.src('static/scripts/**/*.js')
    // pipe() 让文件流顺着管道向下流，支持‘点’操作符
    .pipe(minify())
    // 处理完毕的文件，将会用build替换掉base，即为 build/libs/dialog.js
    .pipe(gulp.dest('build'));

// 手动设置base的值
gulp.src('static/scripts/**/*.js', { base: 'static' })
  .pipe(minify())

  // 使用build替换base，结果为 build/scripts/libs/dialog.js
  .pipe(gulp.dest('build'));
```

## gulp.dest(path[, options])
```javascript
    // 拷贝文件
    gulp.task('dest',function(){
        // 获取src目录下的所有html文件拷贝到dist目录下
        gulp.src('src/*.html').pipe(gulp.dest('dist/'));
    });

    // default
    gulp.task('default',function(){
        // 监听，如若src目录下文件发生变化则执行dest任务
        gulp.watch('src/*',['dest']);
    });

    // command line
    gulp default
```

## gulp.task(name[, deps], fn)

- name 类型：string 任务的名字，如果你需要在命令行中运行你的某些任务，那么不要在任务名字中使用空格
- deps 类型： array 一个包含任务列表的数组，这些任务会在你当前任务运行之前完成

**定义gulp任务**
```javascript
gulp.task('somename', function() {
  // dosomething
})

gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // dosomething
  //请一定要确保你所依赖的任务列表中的任务都使用了正确的异步执行方式：
  // 使用一个 callback，或者返回一个 promise 或 stream。
})

```

- fn 该函数定义任务所需要执行的一些操作。通常来说，它会是这样中形式
```javascript
gulp.src().pipe(someplugin())
```
> 异步任务支持 如果fn能够做到以下其中一点，就可以异步执行了

**接受一个callback**
```javascript
// 在 shell 中执行一个命令
var exec = require('child_process').exec;
gulp.task('jekyll', function(cb) {
  // 编译 Jekyll
  exec('jekyll build', function(err) {
    if (err) return cb(err); // 返回 error
    cb(); // 完成 task
  });
});
```

**返回一个stream**
```javascript
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```
**或者返回一个promise**

```javascript
var Q = require('q');

gulp.task('somename', function() {
  var deferred = Q.defer();

  // 执行异步的操作
  setTimeout(function() {
    deferred.resolve();
  }, 1);

  return deferred.promise;
});
```

> 总结：默认的，task 将以最大的并发数执行，也就是说，gulp 会一次性运行所有的 task 并且不做任何等待。<br />
如果你想要创建一个序列化的 task 队列，并以特定的顺序执行，你需要做两件事：

1. 给出一个提示，来告知 task 什么时候执行完毕 
2. 并且再给出一个提示，来告知一个 task 依赖另一个 task 的完成。

对于这个例子，让我们先假定你有两个 task，"one" 和 "two"，并且你希望它们按照这个顺序执行：

1. a 在 "one" 中，你加入一个提示，来告知什么时候它会完成：可以再完成时候返回一个 callback，<br />
或者返回一个 promise 或 stream，这样系统会去等待它完成。

2. b 在 "two" 中，你需要添加一个提示来告诉系统它需要依赖第一个 task 完成。

因此，这个例子的实际代码将会是这样：

```javascript
var gulp = require('gulp');

// 返回一个 callback，因此系统可以知道它什么时候完成
gulp.task('one', function(cb) {
    // 做一些事 -- 异步的或者其他的
    // 如果 err 不是 null 或 undefined，则会停止执行，且注意，这样代表执行失败了
    cb(err);
});

// 定义一个所依赖的 task 必须在这个 task 执行之前完成
gulp.task('two', ['one'], function() {
    // 'one' 完成后
});

gulp.task('default', ['one', 'two']);
```

**gulp.watch(glob [, options], tasks) 或 gulp.watch(glob [, options, cb])**
> 监视文件，并且可以在文件发生改动时候做一些事情。它总会返回一个 EventEmitter 来发射（emit） change 事件。

- gulp.watch(glob[, options], tasks)
> glob与options略过。<br/>
tasks 类型: 数组 需要在文件变动后执行的一个或者多个通过 gulp.task() 创建的 task 的名字，

```javascript
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

- gulp.watch(glob[, options, cb])
> glob与options略过<br/>
cb(event) 类型： Function 每次变动需要执行的回调函数

```javascript
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});

callback 会被传入一个名为 event 的对象。这个对象描述了所监控到的变动：

event.type 类型: String 发生变动的行为类型: added, changed, deleted

event.path 类型: String 触发该事件的文件路径
```

**转载**
* 原文地址-[gulp排除文件(文件路径匹配模式globs)](http://www.siyuweb.com/gulp/3207.html)



