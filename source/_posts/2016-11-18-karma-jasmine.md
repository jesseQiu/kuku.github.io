---
title: Karma+Jasmine
date: 2016-11-18 17:00:24
updated: 2016-11-18 17:00:24
tags: Jasmine
categories: Testing
feature:
---

本文主要介绍 karma+jasmine 测试环境搭建 Demo

## 一、工程目录
- node_modules: npm 安装的插件
- spec: 测试代码
- src: 代码原文件
- karma.conf.js: karma 的配置文件
- package.json: npm 配置文件
- [完整工程代码下载地址](http://od6sd4xau.bkt.clouddn.com/karma-jasmine-demo.rar)

## 二、package.json
```json
{
  "name": "karma-jasmine-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "karma": "^1.3.0"
  },
  "devDependencies": {
    "jasmine-core": "^2.5.2",
    "karma-chrome-launcher": "^2.0.0",
    "karma-coverage": "^1.1.1",
    "karma-jasmine": "^1.0.2"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "jesse chiu",
  "license": "ISC"
}

```
注意点：如果文件中的 `"name"` 的值为和其中某个插件名称一样，例如：`"name":"karma-jasmine"` 时，在运行下面命令安装该插件时 
```bash
npm install karam-jsmine
```
会报错误：
```bash
npm ERR! Refusing to install karma-jasmine as a dependency of itself
npm ERR!
npm ERR! If you need help, you may report this error at:
npm ERR!     <https://github.com/npm/npm/issues>
npm ERR! Please include the following file with any support request:
npm ERR!     D:\Study\karma\karma-jasmine-demo\npm-debug.log
```
解决办法只要把 `"name"` 的值改为不和要安装的插件名字一样即可，例如：
`"name": "karma-jasmine-demo"`

## 三、karma.conf.js
```js
// Karma configuration
// Generated on Fri Nov 18 2016 14:26:31 GMT+0800 (中国标准时间)

module.exports = function(config) {
  config.set({

    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '',


    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['jasmine'],


    // list of files / patterns to load in the browser
    files: [
        'src/*.js',
        'spec/*.spec.js'
    ],


    // list of files to exclude
    exclude: [
    ],


    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
        'spec/*.spec.js':'coverage'
    },


    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress','coverage'],

    coverageReporter:{
         type:'html',
         dir:'spec/coverage/'
     },


    // web server port
    port: 9876,


    // enable / disable colors in the output (reporters and logs)
    colors: true,


    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,


    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,


    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['Chrome'],


    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false,

    // Concurrency level
    // how many browser should be started simultaneous
    concurrency: Infinity
  })
}

```
该文件可以通过：
```bash
karma init
```
创建。具体过程可以参考下图:
![karma init](http://images0.cnblogs.com/blog2015/455688/201505/291917575947190.png)

## 四、代码

src/add.js

```js
function add(a,b){
	return (a+b);
}
```
spec/add.spec.js

```js
describe("A suite of basic functions", function () {
    it("test", function () {
        expect(3).toEqual(add(1,1));
    });
});
```
## 参考
- [手把手教你如何安装和使用Karma-Jasmine](http://www.cnblogs.com/wushangjue/p/4539189.html?utm_source=tuicool&utm_medium=referral)