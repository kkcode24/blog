# 一些常用的gulp插件

> gulp的插件数量虽然没有grunt那么多，但也可以说是应有尽有了，下面列举一些常用的插件。

## 自动加载插件
> 如果我们要使用的插件非常多，可以使用 `gulp-load-plugins` 把插件自动加载进来

- 安装：
  + ```npm install --save-dev gulp-load-plugins```

要使用的gulp插件比较多的情况下，我们的gulpfile.js文件开头可能就会是这个样子的：

```javascript
var gulp = require('gulp'),

a = require('gulp-a'),
b = require('gulp-b'),
c = require('gulp-c'),
d = require('gulp-d'),
e = require('gulp-e'),
//更多的插件...
z = require('gulp-z');
```

> 虽然这没什么问题，但会使我们的gulpfile.js文件变得很冗长，看上去不那么舒服。<br>
gulp-load-plugins插件正是用来解决这个问题。

gulp-load-plugins这个插件能自动帮你加载package.json文件里的gulp插件。<br>
例如假设你的package.json文件里的依赖是这样的:

```json
{
    "devDependencies": {
        "gulp": "~3.6.0",
        "gulp-rename": "~1.2.0",
        "gulp-ruby-sass": "~0.4.3",
        "gulp-load-plugins": "~0.5.1"
    }
}

```

然后我们可以在gulpfile.js中使用gulp-load-plugins来帮我们加载插件：

```javascript
var gulp = require('gulp');

//加载gulp-load-plugins插件，并马上运行它
var plugins = require('gulp-load-plugins')();

```

然后我们要使用gulp-rename和gulp-ruby-sass这两个插件的时候，就可以使用plugins.rename和plugins.rubySass来代替了,也就是原始插件名去掉gulp-前缀，之后再转换为驼峰命名。

实质上gulp-load-plugins是为我们做了如下的转换

plugins.rename = require('gulp-rename');

plugins.rubySass = require('gulp-ruby-sass');

gulp-load-plugins并不会一开始就加载所有package.json里的gulp插件，而是在我们需要用到某个插件的时候，才去加载那个插件。

最后要提醒的一点是，因为gulp-load-plugins是通过你的package.json文件来加载插件的，所以必须要保证你需要自动加载的插件已经写入到了package.json文件里，并且这些插件都是已经安装好了的。

## 重命名
> 用来重命名文件流中的文件。用gulp.dest()方法写入文件时，文件名使用的是文件流中的文件名。<br>
如果要想改变文件名，那可以在之前用 `gulp-rename` 插件来改变文件流中的文件名。

- 安装：
    + `npm install --save-dev gulp-rename`


```javascript
var gulp = require('gulp'),
rename = require('gulp-rename'),
uglify = require("gulp-uglify");

gulp.task('rename', function () {

    gulp.src('js/jquery.js')
    .pipe(uglify())  //压缩
    .pipe(rename('jquery.min.js')) //会将jquery.js重命名为jquery.min.js
    .pipe(gulp.dest('js'));

    //关于gulp-rename的更多强大的用法请参考https://www.npmjs.com/package/gulp-rename
});
```

## js文件压缩
> 用来压缩js文件，使用的是uglify引擎,插件 `gulp-uglify`

- 安装：
    + `npm install --save-dev gulp-uglify`

```javascript
var gulp = require('gulp'),
uglify = require("gulp-uglify");

gulp.task('minify-js', function () {

    gulp.src('js/*.js') // 要压缩的js文件
    .pipe(uglify())  //使用uglify进行压缩,更多配置请参考：
    .pipe(gulp.dest('dist/js')); //压缩后的路径

});
```

## css文件压缩
> 要压缩css文件时可以使用插件 `gulp-minify-css`

- 安装：
    + `npm install --save-dev gulp-minify-css`


```javascript
var gulp = require('gulp'),
minifyCss = require("gulp-minify-css");

gulp.task('minify-css', function () {

    gulp.src('css/*.css') // 要压缩的css文件
    .pipe(minifyCss()) //压缩css
    .pipe(gulp.dest('dist/css'));

});
```

## html文件压缩

>用来压缩html文件的插件 `gulp-minify-html`

- 安装：
    +  `npm install --save-dev gulp-minify-html`

```javascript
var gulp = require('gulp'),
minifyHtml = require("gulp-minify-html");

gulp.task('minify-html', function () {

    gulp.src('html/*.html') // 要压缩的html文件
    .pipe(minifyHtml()) //压缩
    .pipe(gulp.dest('dist/html'));

});
```

## js代码检查

> 用来检查js代码的插件 `gulp-jshint`

- 安装：
    + `npm install --save-dev gulp-jshint`

```javascript
var gulp = require('gulp'),
jshint = require("gulp-jshint");

gulp.task('jsLint', function () {

    gulp.src('js/*.js')
    .pipe(jshint())
    .pipe(jshint.reporter()); // 输出检查结果

});
```

## 文件合并

> 用来把多个文件合并为一个文件,我们可以用它来合并js或css文件等，这样就能减少页面的http请求数了。<br>
插件 `gulp-concat`

- 安装：
    + `npm install --save-dev gulp-concat`


```javascript
var gulp = require('gulp'),
concat = require("gulp-concat");

gulp.task('concat', function () {

    gulp.src('js/*.js')  //要合并的文件
    .pipe(concat('all.js'))  // 合并匹配到的js文件并命名为 "all.js"
    .pipe(gulp.dest('dist/js'));

});
```

## less和sass的编译
> less使用 `gulp-less` <br>
> sass使用 `gulp-sass`

- 安装：
    + `npm install --save-dev gulp-less`
- 安装：
    + `npm install --save-dev gulp-sass`

```javascript
var gulp = require('gulp'),
less = require("gulp-less"),
sass = require("gulp-sass");

gulp.task('compile-less', function () {

    gulp.src('less/*.less')
    .pipe(less())
    .pipe(gulp.dest('dist/css'));

});

gulp.task('compile-sass', function () {

    gulp.src('sass/*.sass')
    .pipe(sass())
    .pipe(gulp.dest('dist/css'));

});
```

## 图片压缩

> 压缩jpg、png、gif等图片的插件 `gulp-imagemin`

安装：npm install --save-dev gulp-imagemin

```javascript
var gulp = require('gulp');
var imagemin = require('gulp-imagemin');
var pngquant = require('imagemin-pngquant'); //png图片压缩插件

gulp.task('default', function () {

    return gulp.src('src/images/*')
                .pipe(imagemin({
                    progressive: true,
                    use: [pngquant()] //使用pngquant来压缩png图片
                }))
                .pipe(gulp.dest('dist'));

});
```

## css自动添加前缀

> `gulp-autoprefixer`插件,自动can i use 检测css，然后自动添加前缀 

- 安装:
    + `npm install --save-dev gulp-autoprefixer`

```javascript

const gulp = require('gulp');
const autoprefixer = require('gulp-autoprefixer');

gulp.task('default', () =>
	gulp.src('src/app.css')
		.pipe(autoprefixer({
			browsers: ['last 2 versions'],
			cascade: false
		}))
		.pipe(gulp.dest('dist'))
);

```

## 文件名修改成hash值

> `gulp-rev`插件,文件名自动加hash值，来达到清除缓存的目的。

- 安装:
    + `npm install gulp-rev --save-dev`

```javascript

const gulp = require('gulp');
var rev = require('gulp-rev');

gulp.task('rev',function(){
    gulp.src(['./dist/**/*.css','./dist/**/*.js','./dist/**/*'],{base:'./'})
        /* 转换成新的文件名 */
        .pipe(rev())
        .pipe(gulp.dest('./dist'))
        /*收集原文件名与新文件名的关系*/
        .pipe(rev.manifest())
        // 将文件以json的形式存在当前项目下的 ./rev 目录下
        .pipe(gulp.dest('./rev'));
});

```
## 替换文件路径插件

> `gulp-rev-collector`插件

- 安装:
    + `npm install gulp-rev-collector --save-dev`

```javascript

const gulp = require('gulp');
var revCollector = require('gulp-rev-collector');

/* 使用这个模块，需要依赖rev任务，所以需要注入rev任务，如果不注入需要先执行rev任务 */
gulp.task('revCollector',['rev'],function(){
// 根据生成的json文件，去替换html里的路径
gulp.src(['./rev/*.json','./dist/*.html'])
       .pipe(revCollector())
       .pipe(gulp.dest('./dest'));
})

```

## 优化gulp命令
> 从上面看到，我们每每去执行一条任务，就要运行一条命令，这样显然是不合理的，所以我们需要进行优化。<br>
优化的方式就是任务注入以及 return 的使用 

```javascript
var gulp = require('gulp');
var autoprefixer = require('gulp-autoprefixer');
var concat = require('gulp-concat');
var imagemin = require('gulp-imagemin');
var htmlmin = require('gulp-htmlmin');
var rev = require('gulp-rev');
var revCollector = require('gulp-rev-collector');
var uglify = require('gulp-uglify');

// 处理CSS
gulp.task('css', function () {
    return gulp.src('./css/*.css', {base: './'})
               .pipe(autoprefixer())
               .pipe(gulp.dest('./dist'));
});

// 此处就不做CSS压缩的演示了，原理相同。

// 压缩js
gulp.task('js', function() {
    return gulp.src('./js/*.js', {base: './'})
               .pipe(uglify())
               .pipe(gulp.dest('./dist'));
});

// 压缩图片
gulp.task('image', function () {
    return gulp.src('./images/*', {base: './'})
               .pipe(imagemin())
               .pipe(gulp.dest('./dist'));
});

// 压缩html
gulp.task('html', function () {
    return gulp.src('./*.html')
               .pipe(htmlmin({
                   removeComments: true,
                   collapseWhitespace: true,
                   minifyJS: true
               }))
               .pipe(gulp.dest('./dist'));
});

// 生成hash文件名
gulp.task('rev',['css', 'js', 'image', 'html'], function () {
    // 其中加!前缀的是表示过滤该文件
    return gulp.src(['./dist/**/*', '!**/*.html'], {base: './dist'})
               .pipe(rev())
               .pipe(gulp.dest('./dist'))
               .pipe(rev.manifest())
               .pipe(gulp.dest('./rev'));
});

// 替换文件路径
gulp.task('revCollector',['rev'], function () {
    // 根据生成的json文件，去替换html里的路径
    return gulp.src(['./rev/*.json', './dist/*.html'])
               .pipe(revCollector())
               .pipe(gulp.dest('./dist'));
});

// gulp中默认以default为任务名称
gulp.task('default', ['revCollector']);
```

**执行任务,在命令行中，我们只需要执行下面的命令**
```shell
$ gulp
或者是 
$ gulp default 
```

