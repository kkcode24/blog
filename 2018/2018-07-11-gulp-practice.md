# gulp实践记录
> gulp的工作流程就是： 
> 找到文件( gulp.src ) -> 对文件进行操作( 使用各类插件 ) -> 输出文件 ( gulp.dest );

## 配置1

- 源码地址：[下载download](https://github.com/kkcode24/blog/raw/master/2018/zip/gulp.zip)

```
gulp项目根目录
│  gulpfile.js
│  package-lock.json
│  package.json
│
├─app
│  │  index.html
│  │  page.html
│  │
│  ├─css
│  ├─fonts
│  ├─images
│  ├─js
│  │  │  main.js
│  │  │
│  │  └─lib
│  │          a-library.js
│  │          another-library.js
│  │
│  └─scss
│          styles.scss
│          styles2.scss
│
└─dist
```

```javascript
// gulpfile.js
// 引入gulp
var gulp = require('gulp');

//==========引入组件==============================//
var sass = require('gulp-sass');
var browserSync = require('browser-sync');


var useref = require('gulp-useref'); // 合并文件

var uglify = require('gulp-uglify'); // js压缩

var gulpIf = require('gulp-if');
var minifyCSS = require('gulp-minify-css'); // css压缩

var imagemin = require('gulp-imagemin'); // 图片压缩
var cache = require('gulp-cache');

var del = require('del'); // 删除旧文件

var runSequence = require('run-sequence'); // 按照顺序执行任务
//==========引入组件==============================//





/*=====================Development Tasks======================= */
// Start browserSync server
gulp.task('browserSync',function(){
	browserSync({
		server: {baseDir: 'app'}
	})
});

gulp.task('sass',function(){
	return gulp.src('app/scss/**/*.scss')
		   .pipe(sass())
		   .pipe(gulp.dest('app/css'))
		   // 自动刷新浏览器
		   .pipe(browserSync.reload({
		   	stream: true
		   }))
});

// Watchers
gulp.task('watch',['browserSync','sass'],function(){
	gulp.watch('app/scss/**/*.scss',['sass']);
	// Reloads the browser whenever HTML or JS files change
	gulp.watch('app/*.html',browserSync.reload);
	gulp.watch('app/js/**/*.js',browserSync.reload);
});
/*=====================Development Tasks======================= */






/*=====================开发过程和发布过程任务分割线======================= */






/*=====================Optimization Tasks ======================= */

// Optimizing CSS and JavaScrip
gulp.task('useref', function(){
  return gulp.src('app/*.html')
    .pipe(gulpIf('*.css', minifyCSS()))
    .pipe(gulpIf('*.js', uglify()))
    .pipe(useref())
    .pipe(gulp.dest('dist'))
});

// Optimizing Images 
gulp.task('images', function(){
  return gulp.src('app/images/**/*.+(png|jpg|gif|svg)')
  .pipe(cache(imagemin({
      interlaced: true
    })))
  .pipe(gulp.dest('dist/images'))
});

// Copying fonts 
gulp.task('fonts', function() {
  return gulp.src('app/fonts/**/*')
    .pipe(gulp.dest('dist/fonts'))
})

// 在某些时候我们还是需要清除图片，所以clean任务我们还需要保留。
gulp.task('clean', function() {
  return del.sync('dist').then(function(cb) {
    return cache.clearAll(cb);
  });
})

// 这个任务会删除，除了images/文件夹，dist下的任意文件。
gulp.task('clean:dist', function(){
  return del.sync(['dist/**/*', '!dist/images', '!dist/images/**/*'])
});

/*=====================Optimization Tasks ======================= */




// 默认任务是开发过程，编译Sass，监听文件，刷新浏览器。
gulp.task('default', function (callback) {
  runSequence(['sass','browserSync', 'watch'],
    callback
  )
})

// 发布构建任务，优化CSS,JavaScript,压缩图片，并把资源从app移动到dist。
gulp.task('build', function (callback) {
  runSequence('clean:dist',
    ['sass', 'useref', 'images', 'fonts'],
    callback
  )
})

```

## 配置2

```javascript
// 引入 gulp
var gulp = require('gulp');

// 引入组件
var jshint = require('gulp-jshint'); //语法检查
var sass = require('gulp-sass'); //sass编译
var concat = require('gulp-concat'); //合并
var uglify = require('gulp-uglify'); //js压缩
var rename = require('gulp-rename'); //重命名
var htmlmin = require('gulp-htmlmin'); //页面压缩
var minifyCss = require('gulp-minify-css'); //css压缩
var rev = require('gulp-rev'); //对文件名加MD5后缀
var revCollector = require('gulp-rev-collector'); //路径替换
var cheerio = require('gulp-cheerio'); //替换引用


// 默认任务 
gulp.task('default', ['sass', 'lint']);
// 开发环境gulp任务
gulp.task('watch', function() {
    // gulp.watch('./src/index.html', ['sass', 'lint']);
    gulp.watch('./src/scss/*.scss', ['sass']);
    gulp.watch('./src/js/*.js', ['lint']);
});
// 检查脚本 
gulp.task('lint', function() {
    gulp.src('./src/js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});
// 编译Sass 
gulp.task('sass', function() {
    gulp.src('./src/scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./src/css'));
});



// 生产环境gulp任务
gulp.task('dev', ['cssConcat', 'scripts']);
// js合并，压缩文件 
gulp.task('scripts', function() {
    gulp.src('./src/js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist/js'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(rev())
        .pipe(gulp.dest('./dist/js'))
        .pipe(rev.manifest({merge: true}))
        .pipe(gulp.dest('./rev/js/'));
});
// css合并压缩
gulp.task('cssConcat', function() {
    gulp.src('./src/css/**.css')
        .pipe(concat('style.min.css'))
        .pipe(minifyCss())
        .pipe(rev())
        .pipe(gulp.dest('./dist/css'))
        .pipe(rev.manifest({merge: true}))
        .pipe(gulp.dest('./rev/css/'));
});
//改变引用文件
gulp.task('rev', function() {
    gulp.src(['./rev/css/rev-manifest.json', './rev/js/rev-manifest.json', './dist/index.html'])
        .pipe(revCollector()) 
        .pipe(gulp.dest('./dist/'));
});
gulp.task('indexHtml', function() {
    return gulp.src('index.html')
        .pipe(cheerio(function ($) {
            $('script').remove();
            $('link').remove();
            $('body').append('<script src="./js/all.min.js"></script>');
            $('head').append('<link rel="stylesheet" href="./css/style.min.css">');
        }))
        .pipe(gulp.dest('dist/'));
});
//压缩html文件
gulp.task('minify', function() {
    return gulp.src('dist/*.html')
        .pipe(htmlmin({
            collapseWhitespace: true
        }))
        .pipe(gulp.dest('./dist/'));
});
```

## test配置

```
// package.json
"devDependencies": {
    "gulp": "^3.9.1",
    "gulp-autoprefixer": "^5.0.0",
    "gulp-concat": "^2.6.1",
    "gulp-minify-css": "^1.2.4",
    "gulp-uglify": "^3.0.0"
}

```

```javascript
var gulp = require('gulp');
const concat = require('gulp-concat');
const uglify = require('gulp-uglify');

const autoprefixer = require('gulp-autoprefixer');
const minifyCss = require('gulp-minify-css');

gulp.task('default',['script','styles'],function(){
	// 将你的默认的任务代码放在这
	console.log('default run time ...');
	gulp.watch('./src/script/*.js',['script']);
	gulp.watch('./src/styles/*.css',['styles']);
});

gulp.task('script',function(){
	gulp.src('./src/script/*.js')
	    .pipe( concat('ab.js'))
	    .pipe( uglify() )
	    .pipe( gulp.dest('./build/script') );
});


gulp.task('styles',function(){
	gulp.src('./src/styles/*.css')
	    .pipe( concat('styles.css')) 
	    .pipe( autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4') )
	    .pipe( minifyCss() )
	    .pipe( gulp.dest('./build/styles') );
});
```


## test2配置

```

var gulp = require('gulp');
var less = require('gulp-less');
var plugins = require('gulp-load-plugins')();
var pngquant = require('imagemin-pngquant');


gulp.task('default', function(){
    gulp.src('less/zhanzhao.less').pipe(less()).pipe(gulp.dest('css/'));
    gulp.src('less/liuqian.less').pipe(less()).pipe(gulp.dest('css/'));
    gulp.src('less/student.less').pipe(less()).pipe(gulp.dest('css/'));
    return gulp.src('less/company.less').pipe(less()).pipe(gulp.dest('css/'));
});

gulp.task('clean',function(){
    return gulp.src('publish/').pipe(plugins.clean());
});


gulp.task('bulid', ['clean'],function(){
  gulp.src('favicon.ico').pipe(gulp.dest('publish/'));
    gulp.src('download/**/*').pipe(gulp.dest('publish/download/'));
    gulp.src('mail/**/*').pipe(gulp.dest('publish/mail/'));
    gulp.src('statement/**/*').pipe(gulp.dest('publish/statement/'));
    gulp.src('template/**/*').pipe(gulp.dest('publish/template/'));
  gulp.src('css/**/*.css').pipe(plugins.minifyCss()).pipe(gulp.dest('publish/css/'));
    gulp.src('scripts/**/*.js').pipe(plugins.uglify()).pipe(gulp.dest('publish/scripts/'));
  return gulp.src('images/**/*').pipe(plugins.cache(plugins.imagemin({
            optimizationLevel: 5,
            progressive: true,
            svgoPlugins: [{removeViewBox: false}],
            use: [pngquant()]
        }))).pipe(gulp.dest('publish/images/'));
});

gulp.task("revision",['bulid'],function(){
  gulp.src(['template/head-js.html', 'template/baidu.html']).pipe(plugins.concat('head-js.html')).pipe(gulp.dest('publish/template/'));

  return gulp.src(['publish/css/*.css','publish/scripts/config.js','publish/images/**/*'],{base: 'publish'})
        .pipe(plugins.rev())
        .pipe(gulp.dest('publish/'))
        .pipe(plugins.rev.manifest({
          merge: true
        }))
        .pipe(gulp.dest('publish/'));
});


gulp.task("publish", ["revision"],function(){
  var manifestCss = gulp.src("publish/rev-manifest.json"),
      manifestDownload = gulp.src("publish/rev-manifest.json"),
      manifest = gulp.src("publish/rev-manifest.json");

  gulp.src('publish/css/*.css')
    .pipe(plugins.revReplace({manifest: manifest}))
    .pipe(gulp.dest('publish/css/'));
 
  gulp.src('*.html')
    .pipe(plugins.revReplace({manifest: manifestCss}))
    .pipe(gulp.dest('publish/'));

  gulp.src('publish/download/*.html')
    .pipe(plugins.revReplace({manifest: manifestDownload}))
    .pipe(gulp.dest('publish/download/'));
});

```

[三只松鼠干货源码](https://segmentfault.com/a/1190000006157372#articleHeader3)


