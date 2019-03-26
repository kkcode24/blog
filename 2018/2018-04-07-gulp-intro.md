## gulp的一些有用链接
+ [gulp官网](http://gulpjs.com/)
+ [gulp中文网](http://www.gulpjs.com.cn/)
+ [gulp-native-api(包含node-glob语法介绍)](https://github.com/kkcode24/blog/blob/master/2018/2018-04-09-gulp-native-api.md)
+ [一些常用的gulp插件](https://github.com/kkcode24/blog/edit/master/2018/2018-04-13-gulp-plugins.md)
+ [gulp插件网](https://gulpjs.com/plugins/)
+ [gulp源码解析](http://www.cnblogs.com/vajoy/p/6349817.html)

## gulp的安装
1. 全局安装gulp
```
npm install --global gulp
```
2. 作为项目的开发依赖（devDependencies）安装
```
npm install --save-dev gulp
```
3. 在项目根目录下创建一个名为gulpfile.js的文件

```
var gulp = require('gulp');

gulp.task('default',function(){
	// 将你的默认的任务代码放在这
});
```

4 . 运行gulp

```
gulp
```

**注意**
> 默认的名为default的任务将会被运行，在这里，这个任务并未做任何事情。
想要单独执行特定的任务（task），请输入```gulp <task> <othertask>```

## gulp的应用
> gulp 是用来帮助我们执行一些重复操作的
一般我们将这些重复的操作划分为不同的任务

### 如何定义一个 任务

1. 第一个参数是任务名
2. 第二个参数是任务执行体
```
gulp.task('hello',function(){
	console.log(hello kkcode);
});
```

### 运行任务
> gulp hello


### 简单的gulpfile.js文件
```gulpfile.js
var gulp = require('gulp');
var uglify = require('gulp-uglify');
var concat = require('gulp-concat');

var paths = {
	script:['js/index.js','js/main.js']
}

gulp.task('default',function(){
	// paths 也可以正则匹配，如： 'js/*.js'
	gulp.src(paths)
			.pipe(uglify)
			.pipe(concat('all.min.js'))
			.pipe(gulp.dest('build'));
});
```
