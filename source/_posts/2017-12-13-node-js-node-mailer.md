---
title: Node Mailer
date: 2017-12-14 14:34:39
updated: 2137-12-14 14:34:39
categories: Node.js
---
>电子邮件，是互联网应用最广泛使用的服务之一，通过电子邮件系统，我们可以与世界上任何一个角落的网络用户进行联系。

使用 Nodejs 收发电子邮件也非常简单，Nodemailer 包就可以帮助快速实现发邮件的功能。

## 封装实例代码
- 封装代码
```js
/**
 * 发送邮件(目前暂时默认使用 qq smtp 作为邮件服务器)
 * @param  {[type]} configObj [description]
 * {
 *      senderOptions:{
 *          user: xxxx@qq.com,
 *          password: xxxxx
 *      },
 *      mailOptions:{
 *          to:['xxx@qq.com','ooo@qq.com'],
 *          subject:'xxxx',
 *          text:'xxxx',
 *          html:'',
 *          attachments:[{
 *              
 *          }]
 *      }
 * }
 * @return {[type]}           [description]
 */
function sendEmail(configObj) {
    console.log(`sendEmail: ${JSON.stringify(configObj)}`);
    return new Promise((resolve, reject) => {
        // create reusable transporter object using the default SMTP transport
        let transporter = nodemailer.createTransport({
            host: 'smtp.qq.com',
            port: 465,
            secure: true, // true for 465, false for other ports
            auth: {
                user: configObj.senderOptions.user, // 发送邮箱的地址
                pass: configObj.senderOptions.password // 发送邮箱密码
            }
        });
        // setup email data with unicode symbols
        let mailOptions = {
            from: configObj.senderOptions.user, // sender address
            to: configObj.mailOptions.to.join(','), // list of receivers
            subject: configObj.mailOptions.subject, // Subject line
            text: configObj.mailOptions.text, // plain text body
            // 当有 html 内容时,text 内容无效
            html: configObj.mailOptions.html, // html body
            attachments: configObj.mailOptions.attachments
        };

        // send mail with defined transport object
        transporter.sendMail(mailOptions, (error, info) => {
            if (error) {
                console.error(`sendMail fail: ${JSON.stringify(error)}`);
                transporter.close();
                return reject(JSON.stringify(error));
            }
            console.log(`sendMail success: ${JSON.stringify(info)}`);
            transporter.close();
            resolve(info);
        });
    });
}
```
- 调用代码
```js
let emailObj = {
    senderOptions: {
        user: `jessechiu@foxmail.com`,
        password: `xxxxx`
    },
    mailOptions: {
        to: ['jessechiu@foxmail.com', '48593811@qq.com'],
        subject: 'Hello NodeMailer',
        text: 'One today is worth two tomorrows!',
        html: '<p>One today is worth two tomorrows!</p><p> <img src="cid:001"></p>',
        attachments: [{
            // 创建一个文本文件作为附件
            filename: 'HelloNodeMailer.md',
            content: 'One today is worth two tomorrows!'
        },{
            filename: 'scenery.jpg',
            // 支持本地文件和网络文件
            path: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1513761286&di=bd825ca63f37690549ef4588f9974895&imgtype=jpg&er=1&src=http%3A%2F%2Fimgsrc.baidu.com%2Fimgad%2Fpic%2Fitem%2F32fa828ba61ea8d35e5c44059d0a304e251f58b0.jpg'
        }, {
            filename: 'girl.jpg',
            // 本地图片,嵌入到 html 内容页面中
            path: './asserts/girl.jpg',
            cid: '001'
        }]
    }

}
// 发送邮件
util.sendEmail(emailObj)
.then(console.log)
.catch(console.error)
```
## 参考:
- [Nodemailer 官网](https://nodemailer.com/about/)
- [Nodejs发邮件组件Nodemailer](http://blog.fens.me/nodejs-email-nodemailer/)