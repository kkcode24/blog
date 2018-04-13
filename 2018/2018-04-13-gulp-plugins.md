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

## 自动刷新

> gulp-livereload插件,当代码变化时，它可以帮我们自动刷新页面。<br>
该插件最好配合谷歌浏览器来使用，且要安装livereload chrome extension扩展插件,不能下载的请自行FQ。

- 安装:
    + `npm install --save-dev gulp-livereload`

```javascript

var gulp = require('gulp'),
less = require('gulp-less'),
livereload = require('gulp-livereload');

gulp.task('less', function() {

    gulp.src('less/*.less')
        .pipe(less())
        .pipe(gulp.dest('css'))
        .pipe(livereload());

});

gulp.task('watch', function() {
    livereload.listen(); //要在这里调用listen()方法
    gulp.watch('less/*.less', ['less']);
});
```