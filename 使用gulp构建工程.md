# 使用gulp构建工程"

# 介绍
[gulp](http://gulpjs.com/)是一种基于流的，代码优于配置的新一代构建工具。
 为什么使用gulp。大家可以参考[这里](http://segmentfault.com/a/1190000002491282)。

## 安装gulp

``` bash
$ npm install gulp -g
```

## 安装gulp插件

``` bash
//切换进项目根目录中安装jshint插件
$ npm install gulp-jshint --save-dev
//还可以同时下载多个插件
$ npm install gulp-livereload --save-dev
```

gulp社区提供了很多插件如：Sass/Less预编译器，CSS合并压缩，reload等。更多链接[点这里](http://gulpjs.com/plugins/)

## 常用语法分析

### gulp.src()

    //获取文件列表，返回一个文件的stream,方便将其送入pipe进行操作    
    //gulp.src(globs[,options])
    gulp.src('main/*/*.js',{base:'main'}).pipe();

### gulp.dest()

    //将管道数据写入文件夹中
    //gulp.dest(path[,options])
    gulp.src('main/*/*.js').pipe(gulp.dest('main/*/1'));

### gulp.task()

    //定义一个自定义任务
    //gulp.task(name[.deps],fn)
    gulp.task('task-name1',[task-name2,task-name3],function(){});

### gulp.watch()

    //检测文件变化，通过emits一个change事件触发EventEmitter
    //gulp.watch(glob [, opts], tasks)
    //gulp.watch(glob [, opts, cb])
    var watcher = gulp.watch('main/*/*.js',[task-name1,task-name1]);
    watcher.on('change',function(e){});


## 加载插件

在根目录下创建gulpfile.js文件，并在其中编写加载方法

    var gulp = require('gulp'),
        jshint = require('gulp-jshint'),
        livereload = require('gulp-livereload');   

## 创建任务

    gulp.watch('main/*/*.js',function(){
        livereload.listen();
    })
    gulp.task('scripts',function(){
        return gulp.src('main/*/*.js').pipe(jshint());
    })

## 运行 gulp

``` bash
$ gulp
```
