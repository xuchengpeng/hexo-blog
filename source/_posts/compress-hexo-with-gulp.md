---
title: 使用 gulp 压缩 hexo 博客资源
date: 2018-02-01 14:06:32
udpated: 2018-02-08 17:40:32
comments: true
categories:
 - Technology
 - Hexo
tags:
 - Hexo
 - gulp
---

![](/images/gulp.jpg)

使用 `hexo generate` 命令后在 public 文件夹下面生成 html、css、js 等源文件，里面含有大量的空白，而且博客中还有大量的图片占据了很大的空间。

>Automate and enhance your workflow.
>gulp is a toolkit for automating painful or time-consuming tasks in your development workflow, so you can stop messing around and build something.

[gulp](https://gulpjs.com/) 用自动化构建工具增强你的工作流程！

我们可以通过一些 gulp 插件实现对html、css、js、image等静态资源的高效压缩，通过压缩这些静态资源，可以减少请求的数据量从而达到优化博客访问速度的目的。
<!-- more -->
## 安装 gulp 及插件

* 基本：[gulp](https://www.npmjs.com/package/gulp)
* CSS 压缩：[gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css)
* JS 压缩：[gulp-uglify](https://www.npmjs.com/package/gulp-uglify)
* HTML 压缩：[gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin)，[gulp-htmlclean](https://www.npmjs.com/package/gulp-htmlclean)
* 图片压缩：[gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)

```sh
$ npm install gulp-clean-css gulp-uglify gulp-htmlmin gulp-imagemin gulp-htmlclean gulp --save-dev
```

如果使用 {% post_link autodeploy-hexo-blog-with-travisci Travis CI 自动部署 hexo 博客 %}，`package.json`文件可如下定义：
```json
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": "3.5.0"
  },
  "dependencies": {
    "hexo": "^3.5.0",
    "hexo-deployer-git": "^0.3.1",
    "hexo-generator-archive": "^0.1.5",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-feed": "^1.2.2",
    "hexo-generator-index": "^0.2.1",
    "hexo-generator-searchdb": "^1.0.8",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.1",
    "hexo-renderer-marked": "^0.3.2",
    "hexo-renderer-stylus": "^0.3.3",
    "hexo-server": "^0.3.1",
    "hexo-symbols-count-time": "^0.3.2"
  },
  "devDependencies": {
    "gulp": "^3.9.1",
    "gulp-clean-css": "^3.9.2",
    "gulp-htmlclean": "^2.7.16",
    "gulp-htmlmin": "^4.0.0",
    "gulp-imagemin": "^4.1.0",
    "gulp-uglify": "^3.0.0"
  }
}
```

## 编写 gulpfile.js

在 hexo 博客根目录创建 `gulpfile.js` 文件，可根据自身需求修改相关参数，内容如下：
```js
var gulp = require('gulp');
var minifycss = require('gulp-clean-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');

// 压缩html
gulp.task('minify-html', function() {
    return gulp.src('./public/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true,
            minifyJS: true,
            minifyCSS: true,
            minifyURLs: true,
        }))
        .pipe(gulp.dest('./public'))
});

// 压缩css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss({
            compatibility: 'ie8',
            rebase: false,
        }))
        .pipe(gulp.dest('./public'));
});

// 压缩js
gulp.task('minify-js', function() {
    return gulp.src('./public/js/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});

// 压缩图片
gulp.task('minify-images', function() {
    return gulp.src('./public/images/**/*.*')
        .pipe(imagemin(
        [imagemin.gifsicle({'optimizationLevel': 3}), 
        imagemin.jpegtran({'progressive': true}), 
        imagemin.optipng({'optimizationLevel': 7}), 
        imagemin.svgo()],
        {'verbose': true}))
        .pipe(gulp.dest('./public/images'))
});

// 默认任务
gulp.task('default', [
    'minify-html','minify-css','minify-js','minify-images'
]);
```

## 压缩生成的博客文件

```sh
$ hexo clean
$ hexo generate
$ gulp
```

压缩完成后，执行 `hexo deploy` 部署即可。

如果使用 {% post_link autodeploy-hexo-blog-with-travisci Travis CI 自动部署 hexo 博客 %}，`.travis.yml`文件可如下定义：
```yml
language: node_js  #设置语言

node_js: stable  #设置相应的版本

notifications:
  email: # 构建完成后邮件通知
    recipients:
      - 330476629@qq.com
      - xucp@outlook.com
    on_success: always # default: change
    on_failure: always # default: always

install:
  - npm install  #安装hexo及插件

script:
  - hexo clean  #清除
  - hexo generate  #生成
  - gulp #压缩

after_script:
  - cd ./public
  - git init
  - git config user.name "xuchengpeng"  #修改name
  - git config user.email "330476629@qq.com"  #修改email
  - git add .
  - git commit -m "Site updated"
  - git push --force --quiet "https://${TRAVIS_TOKEN}@${GH_REF}" master:master  #TRAVIS_TOKEN是在Travis中配置token的名称

branches:
  only:
    - hexo  #只监测hexo分支，hexo是我的分支的名称，可根据自己情况设置

env:
 global:
   - GH_REF: github.com/xuchengpeng/xuchengpeng.github.io.git # GitHub博客仓库的地址
```
