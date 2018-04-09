# gulp-api

## gulp.src(globs[, options])
> 输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 
将返回一个 Vinyl files 的 stream 它可以被 piped 到别的插件中。

```javascript

    // 拷贝文件
    gulp.task('dest',function(){
        // 获取src目录下的所有html文件拷贝到dist目录下
        gulp.src('src/*.html').pipe(gulp.dest('dist/'));
    });

    // 递归获取src目录下的所有文件及文件夹及文件下的所有文件
    gulp.src('src/**/*.*');

    // pipe() 让文件流顺着管道向下流，支持‘点’操作符
```

### Glob Primer

[node-glob 语法](https://github.com/isaacs/node-glob#glob-primer);

```javascript

    // default
    gulp.task('default',function(){
        // 监听，如若src目录下文件发生变化则执行dest任务
        gulp.watch('src/*',['dest']);
    });

    // command line
    gulp default
```



> gulp原本不支持任何功能，只提供最基础的Api