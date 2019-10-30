---
title: 猜数字游戏
date: 2018-04-15 09:49:42
updated: 2018-04-15 09:49:42
categories: JavaScript
---

扫雷游戏想必大家都玩过，今天我向大家介绍一个类似扫雷的游戏，**猜数字** 游戏。

先来两张靓照:
![](http://images.jessechiu.com/guess-number-1.png)
![](http://images.jessechiu.com/guess-number-2.png)

具体的游戏规则:
1. 每次随机生成 5 个数字(1-100 之间，游戏玩家看不到)
2. 用户通过在输入框中输入可能的数字，进行猜测
3. 每猜一次，游戏会自动记录，直到你猜的数字是第一步生成的 5 个数字之一，这时游戏结束，最后会显示你总共尝试了多少次

使用到的技术:
- html
- css
- javascript


下面就和大家介绍具体实现步骤:

## 一、页面的主题框架
- html 部分
```html
<div class="container">
    <div class="title">
        <h1>Guess Number</h1>
        <h4>Please enter 1 - 100 number</h4>
    </div>
    <div class="body">
        <label >Please Enter Number: </label><input type="number" id="inputNum" min="0" max="100" name="" />
        <button id="confirm">Confrim</button>
        <hr>
        <div class="result" id="result">
            
        </div>
        <button class="restart" id="restart">Play Again</button>
    </div>
</div>
```
- css 部分
```css
html,body{
    width: 100%;
    height: 100%;
    background:-webkit-gradient(linear, 0 0, 0 bottom, from(#5E07F6), to(rgba(0, 0, 255, 0.5)));  
}
.container{
    margin: 10px auto;
    text-align: center;
    color: #22FD14;
}
.title{
    margin-bottom: 50px;
}
.body button{
    width: 100px;
}
.body input{
    width: 300px;
}
.body hr{
    width: 80%;
    margin-top:20px;
}
#restart{
    width: 200px;
    height: 30px;
    border-color: #65F6E6;
    background-color: #65F6E6;
    display: none;
    margin: 10px auto;
}
@media (max-width: 576px) {
    #confirm{
        margin-top: 5px;
    }
}
```

## 二、js 部分
- 生成随机数
```js
function generatorNum(num){
    // 使用了 es6 set，确保存储的随机数不重复
    let result = new Set();
    while(true){
        if(result.size >= num) break;
        result.add(Math.ceil(Math.random() * 100));
    }   
    console.log(JSON.stringify(Array.from(result)));
    return Array.from(result);
}
```
- 校验输入的数字
```js
function checkNum(){
    if(!inputNumEle.value){
      console.log(`请输入数字!`);
      return;
    }
    if(guessNum.includes(Number(inputNumEle.value))){
      result.innerHTML = `<h3>Lucky Number: ${JSON.stringify(guessNum)}</h3>
      <h4>Congratulations.After ${guessTimes} attempts,you got it!</h4>`
      restartEle.style = 'display:block';
      console.log(`恭喜你在经过 ${guessTimes} 次努力后，终于猜对了`);
    }else{
      guessTimes++;
      // 清除输入值
      inputNumEle.value = '';
      // 设置焦点
      inputNumEle.focus();
      console.log(`没有猜对，再来一次`);
    }
}
```

## 三、完整代码下载
- [Guess Number](https://github.com/Jesse-Chiu/guess-number.git)
- [在线试玩](https://jesse-chiu.github.io/guess-number/)




