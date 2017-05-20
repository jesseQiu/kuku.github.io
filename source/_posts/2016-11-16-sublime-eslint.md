---
title: Sublime ESlint 插件配置
date: 2016-11-16 10:01:11
updated: 2016-11-16 10:01:11
tags: Sublime
categories: Tools
feature:
---

### 一、插件安装
sublime 集成 ESlint 需要全局安装 [ESlint](https://github.com/eslint/eslint) 和两个插件 [SublimeLinter](https://github.com/SublimeLinter/SublimeLinter3)、[SublimeLinter-contrib-eslint ](https://github.com/roadhump/SublimeLinter-eslint)才能正常使用

（插件目录名是 SublimeLinter3，默认 gutter_theme 路径配置是 SublimeLinter，这两个需要保持统一，否则启动时会报错）

然后通过 Preferences->Package Settings->SublimeLinter->Settings - User 进行集成：

```js
{
    "user": {
        "debug": false,
        "delay": 0.25,
        "error_color": "D02000",
        //SublimeLinter与插件文件夹名保持一致
        "gutter_theme": "Packages/SublimeLinter/gutter-themes/Default/Default.gutter-theme",
        "gutter_theme_excludes": [],
        "lint_mode": "background",
        "linters": {
            //新增的
            "eslint": {
                "@disable": false,
                "args": [],
                "excludes": []
            }
        },
        "mark_style": "outline",
        "no_column_highlights_line": false,
        "passive_warnings": false,
        "paths": {
            "linux": [],
            "osx": [],
            "windows": [
                //全局安装 ESLint 生成的 eslint.cmd 的目录，根据自身情况修改
                "**/nodejs/eslint.cmd"
            ]
        },
        "python_paths": {
            "linux": [],
            "osx": [],
            "windows": []
        },
        "rc_search_limit": 3,
        "shell_timeout": 10,
        "show_errors_on_save": false,
        "show_marks_in_minimap": true,
        "syntax_map": {
            "html (django)": "html",
            "html (rails)": "html",
            "html 5": "html",
            "javascript (babel)": "javascript",
            "magicpython": "python",
            "php": "html",
            "python django": "python"
        },
        "warning_color": "DDB700",
        "wrap_find": true
    }
}
```

### 二、添加配置文件

将下面的配置代码保存到根目录，文件名为`.eslintrc.js/json`。也可以参考 [官方文档](http://eslint.org/docs/rules/)。

*建议使用 [standardjs](http://standardjs.com/)*

```js
{
  "globals": {
    "$": true,                                //zepto
    "define": true,                           //requirejs
    "require": true                           //requirejs
  },
  "env": {
    "browser": true                           //执行环境 浏览器
  },
  "rules": {
    //官方文档 http://eslint.org/docs/rules/
    //警告
    // "quotes": [0, "single"],                  //建议使用单引号
    // "no-inner-declarations": [0, "both"],     //不建议在{}代码块内部声明变量或函数
    "no-extra-boolean-cast": 1,               //多余的感叹号转布尔型
    "no-extra-semi": 1,                       //多余的分号
    "no-extra-parens": 1,                     //多余的括号
    "no-empty": 1,                            //空代码块
    "no-use-before-define": [1, "nofunc"],    //使用前未定义
    "complexity": [1, 10],                    //圈复杂度大于10 警告

    //常见错误
    "comma-dangle": [2, "never"],             //定义数组或对象最后多余的逗号
    "no-debugger": 2,                         //debugger 调试代码未删除
    "no-constant-condition": 2,               //常量作为条件
    "no-dupe-args": 2,                        //参数重复
    "no-dupe-keys": 2,                        //对象属性重复
    "no-duplicate-case": 2,                   //case重复
    "no-empty-character-class": 2,            //正则无法匹配任何值
    "no-invalid-regexp": 2,                   //无效的正则
    "no-func-assign": 2,                      //函数被赋值
    "valid-typeof": 2,                        //无效的类型判断
    "no-unreachable": 2,                      //不可能执行到的代码
    "no-unexpected-multiline": 2,             //行尾缺少分号可能导致一些意外情况
    "no-sparse-arrays": 2,                    //数组中多出逗号
    "no-shadow-restricted-names": 2,          //关键词与命名冲突
    "no-undef": 2,                            //变量未定义
    "no-unused-vars": 2,                      //变量定义后未使用
    "no-cond-assign": 2,                      //条件语句中禁止赋值操作
    "no-native-reassign": 2,                  //禁止覆盖原生对象

    //代码风格优化
    "no-else-return": 1,                      //在else代码块中return，else是多余的
    "no-multi-spaces": 1,                     //不允许多个空格
    "key-spacing": [1, {"beforeColon": false, "afterColon": true}],//object直接量建议写法 : 后一个空格前面不留空格
    "block-scoped-var": 2,                    //变量应在外部上下文中声明，不应在{}代码块中
    "consistent-return": 2,                   //函数返回值可能是不同类型
    "accessor-pairs": 2,                      //object getter/setter方法需要成对出现
    "dot-location": [2, "property"],          //换行调用对象方法  点操作符应写在行首
    "no-lone-blocks": 2,                      //多余的{}嵌套
    "no-empty-label": 2,                      //无用的标记
    "no-extend-native": 2,                    //禁止扩展原生对象
    "no-floating-decimal": 2,                 //浮点型需要写全 禁止.1 或 2.写法
    "no-loop-func": 2,                        //禁止在循环体中定义函数
    "no-new-func": 2,                         //禁止new Function(...) 写法
    "no-self-compare": 2,                     //不允与自己比较作为条件
    "no-sequences": 2,                        //禁止可能导致结果不明确的逗号操作符
    "no-throw-literal": 2,                    //禁止抛出一个直接量 应是Error对象
    "no-return-assign": [2, "always"],        //不允return时有赋值操作
    "no-redeclare": [2, {"builtinGlobals": true}],//不允许重复声明
    "no-unused-expressions": [2, {"allowShortCircuit": true, "allowTernary": true}],//不执行的表达式
    "no-useless-call": 2,                     //无意义的函数call或apply
    "no-useless-concat": 2,                   //无意义的string concat
    "no-void": 2,                             //禁用void
    "no-with": 2,                             //禁用with
    "space-infix-ops": 2,                     //操作符前后空格
    "valid-jsdoc": [2, {"requireParamDescription": true, "requireReturnDescription": true}],//jsdoc
    "no-warning-comments": [2, { "terms": ["todo", "fixme", "any other term"], "location": "anywhere" }],//标记未写注释
    "curly": 1                                //if、else、while、for代码块用{}包围
  }
}
```

三、重启 sublime

效果图：   
![这里写图片描述](http://img.blog.csdn.net/20151104150321655)

### 四、gulp 搭建 eslint

安装 gulp-lint 可以生成报表到 checkstyle 目录。

```hljs
gulp.task('lint', function() {
    var d = new Date;
    //yyyy-MM-dd_HHmmss
    var outputTime = d.toLocaleDateString().replace(/[/]/g, '-') + '_' + d.toTimeString().replace(/[:]|\s.+/g, '');
    return gulp.src(paths.src + '/js/**/*.js')
        .pipe(eslint())
        .pipe(eslint.format('html', fs.createWriteStream('../checkstyle/' + outputTime + '.html')))
        .pipe(eslint.format('stylish', process.stdout))
        .pipe(eslint.failAfterError());
});
```

### 参考
- [ESLint 官网](http://eslint.org/)
- [sublime 集成 ESLint](http://blog.csdn.net/lj745280746/article/details/49658249)
- [Sublime Text 中配置 ESLint](http://www.jianshu.com/p/e826e13c67ec)