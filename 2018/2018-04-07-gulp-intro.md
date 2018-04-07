# 自动化工具gulp

## 当下的前端开发

- 不再是简单的使用HTML+CSS+js 这些技术构建网页应用
- 我们要提高效率，就必须减少重复的工作
- 使用less之类预处理的css coffeescript
- 提供开发阶段的便利，开发阶段更快捷
- 现在的开发行业优质的开发人员是不应该将精力放在这些重复性的工作上
- gulp就是一种可以自动化完成我们开发过程中大量的重复工作
	+ 预处理语言的编译
	+ js css html 压缩混淆
	+ 图片体积优化
- 除gulp之外还有类似的自动化工具
	+ grunt
	+ webpack


## gulp的介绍
用自动化构建工具增强你的工作流程！
- 当下最流行的的自动化工具
	+ 什么是自动化工具？
	+ 自动完成一系列重复的操作
- 链接：
	+ [官网](http://gulpjs.com/);
	+ [中文网](http://www.gulpjs.com.cn/);
### 易于使用
> 通过代码优于配置的策略，Gulp 让简单的任务简单，复杂的任务可管理。
### 构建快速
> 利用 Node.js 流的威力，你可以快速构建项目并减少频繁的 IO(input,output) 操作
### 插件高质
> Gulp 严格的插件指南确保插件如你期望的那样简洁高质得工作

### 易于学习
通过最少的 API，掌握 Gulp 毫不费力，构建工作尽在掌握：如同一系列流管道。


## gulp的安装
1. 全局安装gulp
```
npm install --global gulp
// 换镜像
npm config set registry https://registry.npm.taobao.org 
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
4. 运行gulp
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
