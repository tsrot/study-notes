# 模仿知乎界面的一个简单React demo

博客地址：http://blog.xieliqun.com/2016/11/04/react-zhihu/



> 这是一个模仿知乎界面的简单React demo。这个React demo能让你从零开始学习React，并逐渐掌握React。它包括了一个项目从零到项目完成的整个过程。

项目地址：[https://github.com/tsrot/react-zhihu](https://github.com/tsrot/react-zhihu)
项目预览：[http://blog.xieliqun.com/react-zhihu/](http://blog.xieliqun.com/react-zhihu/)

**项目运行**
```javascript
$ git clone https://github.com/tsrot/react-zhihu.git
$ cd react-zhihu

$ npm install

$ bower install

$ gulp server   //用浏览器打开 localhost:5000
```


## 搭建开发环境

### 初始化npm bower

```javascript
npm init  //一直enter，默认就好

bower init //同上

```

### 安装必要的开发工具包

- gulp ：基于流的自动化构建工具
- gulp-browserify ：前端模块及依赖管理
- gulp-concat ：文件合并插件
- gulp-react ：JSX语法转化工具
- gulp-connect ：构建本地开发Web服务器
- lodash ：一个具有一致接口、模块化、高性能等特性的 JavaScript 工具库
- reactify ：React 编译器

```javascript
npm install gulp gulp-browserify gulp-concat gulp-react gulp-connect lodash reactify --save-dev
```

### 安装生产环境依赖包

- react ：主要框架
- react-dom ：React的DOM操作类
- bootstrap ：bootstrap样式

```javascript
npm install --save react react-dom

bower install --save bootstrap
```

### 写入gulp配置文件gulpfile.js

你可以在npm的网站上找到相应插件的gulp配置写法。我配置的[gulpfile.js](https://github.com/tsrot/)


## 开发

- 切分相应的模块
- 分清UI组件和容器组件
- 学会如何在组件之间通信
- 注意写作规范和开发细节

## 部署生产

请切换分支到 product 分支

### 修改gulpfile文件
```javascript
//添加copy任务
gulp.task('copy',function(){
    gulp.src('./app/css/*')
    .pipe(gulp.dest('./dist/css'));

    gulp.src('./bower_components/**/*')
    .pipe(gulp.dest('./dist/libs'));

    gulp.src('./*.html')
    .pipe(gulp.dest('./dist'));
});

//生产服务器
gulp.task('connect-pro',function(){
    connect.server({
        root:'./dist',
        port:port,
        livereload:true,
    })
});

//添加build任务
gulp.task('build',['browserify','copy']);

//添加启动生产服务器任务
gulp.task('server-pro',['build','connect-pro','watch']);
```

### 修改index.html引用目录
```javascript
<link rel="stylesheet" href="./libs/bootstrap/dist/css/bootstrap.css">
<link rel="stylesheet" href="./css/index.css">

<script src="./js/main.js"></script>
```

### 使用gulp-gh-pages部署到github pages

下载gulp-gh-pages插件
```javascript
npm install --save-dev gulp-gh-pages
```
在gulpfile文件中添加配置gulp-gh-pages代码
```javascript
var ghPages = require('gulp-gh-pages');

gulp.task('deploy', function() {
  return gulp.src('./dist/**/*')
    .pipe(ghPages());
});
```

## 后续

将在分支中更新使用下列技术的版本
- webpack
- webpack + redux
- webkack + redux + react-router