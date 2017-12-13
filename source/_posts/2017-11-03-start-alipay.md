---
title: Start AliPay
date: 2017-11-03 10:16:29
categories: AliPay
---

> 本文主要是记录从零开始，配置和搭建如何获取[蚂蚁金服开票平台](https://docs.open.alipay.com/)提供的获取支付宝中发票管家的发票抬头信息过程备忘。

## 一、注册开发平台
### 1. 注册
- 登入[官网](https://open.alipay.com/developmentAccess/developmentAccess.htm)，按照页面的提示步骤。
- 注册
- 创建应用
我这边选择的是 `网页&移动应用` -> `自定义接入`  -> `自用型应用` -> `填写应用名称` -> `添加详细信息` -> `开发配置` -> `提交审核`
开发环境配置中，生成密钥方式可以使用官方提供的工具，直接生成。
![alipay-config01](http://images.jessechiu.com/alipay-config01.png)
![alipay-config02](http://images.jessechiu.com/alipay-config02.png)
- 开发测试
我这一步直接省了，直接跳转到下一步
- 审核发布

## 二、后台服务器配置
由于我使用的是 Node.js 后台，官方没有提供 demo 源码，只能自己撸了。还好在 github 上找到了类似的基础库。
### 1. 下载 [Alipay Node.js SDK 基于最新版蚂蚁金服开发文档](https://github.com/Luncher/alipay)
由于找到的工程是订单相关的，没有我需要的获取发票抬头信息接口，所以接着就是改造代码。由于基础的接口已经实现，所以还是省去了很多工作量。
### 2. 提取工程代码到你现有的工程中
这边主要是增加了一些获取发票抬头信息的接口代码
```js
/**
 * 获取授权 url 地址
 * @param  {[type]} params [description]
 * @return {[type]}        [description]
 * {
 *      redirect_uri:'http://wechatlpt.xxxx.com/xxx/scan-invoice/',
 *      scope: 'auth_invoice_info'
 * }
 */
getAuthUri(params) {
    console.log(`getAuthUri(${JSON.stringify(params)})`);
    let authUri = `${config.aliPayConfig.AUTH_URI}?${querystring.stringify(params)}`;
    console.log(`getAuthUri result: ${authUri}`)
    return authUri;
}

/**
 * 获取 auth token
 * @param  {[type]} params [description]
 * @return {[type]}        [description]
 *  {"app_id":"2017103009616680","source":"alipay_wallet","scope":"auth_invoice_info","auth_code":"c8f168d4ca33480fbe016880e00dXX79"}
 */
getAuthToken(publicParams, basicParams = {}) {
    console.log(`getAuthToken(${JSON.stringify(publicParams)},${JSON.stringify(basicParams)})`)
    return Promise.resolve()
        .then(() => {
            return this.validateParams(config.methodType.GET_AUTH_TOKEN, publicParams, basicParams)
                .then(params => {
                    return this.makeRequest(params)
                })
        })
        .catch(err => ({
            code: '-1',
            message: err.message,
            data: {}
        }))
}

/**
 * 请求发票抬头信息
 * [alipay.ebpp.invoice.title.list.get](https://docs.open.alipay.com/api_36/alipay.ebpp.invoice.title.list.get)
 * @param  {[type]} publicParams [description]
 * @param  {Object} basicParams  [description]
 * @return {[type]}              [description]
 */
queryInvoiceTitle(publicParams, basicParams = {}) {
    return Promise.resolve()
        .then(() => {
            if (!publicParams.user_id) {
                throw new Error("userID can not empty!!!")
            }
            return this.validateParams(config.methodType.QUERY_INVOICE_TITLE, publicParams, basicParams)
                .then(params => {
                    return this.makeRequest(params)
                })
        })
        .catch(err => ({
            code: '-1',
            message: err.message,
            data: {}
        }))
}
```

## 填坑
- android 版本的 支付宝 浏览器不支持 ES6 语法(调了好久才发现这个坑)

## 参考
- [Alipay Node.js SDK 基于最新版蚂蚁金服开发文档](https://github.com/Luncher/alipay)
- [蚂蚁金服开放平台](https://open.alipay.com)

