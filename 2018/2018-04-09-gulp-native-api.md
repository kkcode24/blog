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

[node-glob 语法](https://github.com/isaacs/node-glob#glob-primer);

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

## gulp.src(globs[, options])
> 输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 
将返回一个 Vinyl files 的 stream 它可以被 piped 到别的插件中。

```javascript

    // 拷贝文件
    gulp.task('dest',function(){
        // 获取src目录下的所有html文件拷贝到dist目录下
        gulp.src('src/*.html').pipe(gulp.dest('dist/'));
    });

    // pipe() 让文件流顺着管道向下流，支持‘点’操作符

    // default
    gulp.task('default',function(){
        // 监听，如若src目录下文件发生变化则执行dest任务
        gulp.watch('src/*',['dest']);
    });

    // command line
    gulp default
```



