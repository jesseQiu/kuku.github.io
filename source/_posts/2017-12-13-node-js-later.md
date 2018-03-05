---
title: Lter 
date: 2017-12-13 14:34:39
updated: 2137-12-01 14:34:39
categories: Node.js
---

> Later 是一个基于 Nodejs 的工具库，用最简单的方式执行定时任务。Later 可以运行在 Node 和浏览器中 

## 一、代码封装实例
```js
/**
 * 执行计划表
 * @param  {schedule}   schedule 计划表
 * @param  {Number}   type 0: 无限循环执行计划表，1:只执行一次计划表
 * @param  {Function} executeFun 计划表执行的函数 
 */
function executeSchedule(schedule,type,executeFun){
  console.log(`executeSchedule,(${JSON.stringify(schedule)},${type})`);
  console.log(new Date().toLocaleString());
  // 按照本地时间执行
  later.date.localTime();
  // 打印最近 10 次执行时间表
  let occurrences = later.schedule(schedule).next(10);
  occurrences.forEach((item,index)=>{
    console.log(`${index}: ${item}`);
  });

  let timeHandle = null;
  if(type){
    // 只执行一次计划表
    timeHandle = later.setTimeout(()=>{
      executeFun(timeHandle)
    }, schedule);
  }else{
    // 无限循环执行计划表
    timeHandle = later.setInterval(()=>{
      executeFun(timeHandle)
    }, schedule);
  }
}

/**
 * 每隔 2 秒打印一次日志
 * @return {[type]} [description]
 */
function test1(){
    let sched = later.parse.text('every 2 second');
    util.executeSchedule(sched,0,(timeHandle)=>{
        console.log(new Date().toLocaleString());
    });
}


/**
 * 每日 14:45 和 14：46 执行回调函数
 * 排除，周六和周日
 * @return {[type]} [description]
 */
function test2(){
    var sched = {
        // 每日 14:45 和 14：46 执行计划表
        schedules: [{h: [14], m: [45,46]}],
        // 每周六、日 时间: 注意这里的 7:表示周六 1:表示周日
        // 每周格式 [1-7](周日、周一、周二、... 周六)
        exceptions: [{dw:[7,1]}]
    }
    util.executeSchedule(sched,0,(timeHandle)=>{
        console.log(new Date().toLocaleString());
    });
}
```

## 二、时间格式设置
- Second, s: 秒, 取值范围:[0-59]
- minute, m：分, 取值范围:[0-59]
- hour, h: 时, 取值范围:[0-23]
- time, t: 秒每日, 取值范围:[0-86399]
- day, D: 日, 取值范围:[1-31]
- dayOfWeek, dw, d: 日每周, 取值范围:[1-7](周日、周一、周二、... 周六)
- dayOfYear, dy: 日每年，取值范围:[1-365]
- weekOfMonth, wm: 周每月，取值范围:[1-5]
- weekOfYear, wy: 周每年，取值范围:[1-52]
- month, M: 月，取值范围:[1-12]
- year, Y: 年，取值范围:[1970-2099]


## 参考:
- [later 官网](http://bunkat.github.io/later/index.html)
- [让 Nodejs 来管理定时任务 later](http://blog.fens.me/nodejs-cron-later/)