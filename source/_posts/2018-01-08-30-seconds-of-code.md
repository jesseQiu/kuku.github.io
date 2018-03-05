---
title: 30 Seconds of code
date: 2017-12-21 14:34:39
updated: 2017-12-21 14:34:39
categories: Vue
---
>Curated collection of useful JavaScript snippets that you can understand in 30 seconds or less.

## 一、Date
### 1. 解析时间数组为人类可读字符
技术点:
- [Object.entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)
- `Math.floor()`
- `Array.filter()`
- `Array.map()`
- `Array.join()`

```js
const formatDuration = ms => {
  if (ms < 0) ms = -ms;
  const time = {
    day: Math.floor(ms / 86400000),
    hour: Math.floor(ms / 3600000) % 24,
    minute: Math.floor(ms / 60000) % 60,
    second: Math.floor(ms / 1000) % 60,
    millisecond: Math.floor(ms) % 1000
  };

  // object 转为数组
  let temp = Object.entries(time);
  console.log(`\n Object.entries: ${JSON.stringify(temp)}`);
  // 过滤掉值为 0 的值
  temp = temp.filter(val => val[1] !== 0);
  console.log(`\n filter: ${JSON.stringify(temp)}`);

  // 拆分数组里面的子数组 -> 一维 数组
  temp = temp.map(val => val[1] + ' ' + (val[1] !== 1 ? val[0] + 's' : val[0]));
  console.log(`\n map: ${JSON.stringify(temp)}`);

  // 数组 -> 字符串
  temp = temp.join(', ');
  console.log(`\n join: ${JSON.stringify(temp)}`);
  return temp;
};

let res1 = formatDuration(1001); // '1 second, 1 millisecond'
let res2 = formatDuration(34325055574); // '397 days, 6 hours, 44 minutes, 15 seconds, 574 milliseconds'
console.log(res1,res2);
```

### 2. 计算两个时间间隔多少天
```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) =>
  (dateFinal - dateInitial) / (1000 * 3600 * 24);

let difDay = getDaysDiffBetweenDates(new Date('2017-12-13'), new Date('2017-12-22')); 
console.log(difDay)
```
### 3. 明天
```js
const tomorrow = () => new Date(new Date().getTime() + (1000*3600*24)).toISOString().split('T')[0];
console.log(`tomorrow() ${tomorrow()}`)
```

## 二、Array
### 1. 提取两个数字中不重复值
```js
const union = (a, b) => Array.from(new Set([...a, ...b]));
let res = union([1, 2, 3], [4, 3, 2]);
console.log(`union: ${res}`)
```


## 三、Browser
### 1. 测试当前环境平台
```js
const detectDeviceType = () =>
  /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)
    ? 'Mobile'
    : 'Desktop';

console.log(detectDeviceType())
```

## 字谜
技术点:
- `String.split()`
- `Array.contact()`
- `Array.reduce()`
- `Array.filter()`
- `Array.map()`
- `Array.join()`

```js
let anagrams = str => {
    // 如果只有两个字符,直接对调返回
    if (str.length <= 2) 
        return str.length === 2 ? [str, str[0] + str[1]] : [str];

    // 多余两个，分割为数组
    let strArray = str.split('');
    console.log(`strArray: `,strArray);

    // 再使用数组的 reduce 累加拼接
    return strArray.reduce((acc, letter, i) => {

        console.log(`acc:${JSON.stringify(acc)} letter:${letter} i:${i}`);
    
        let first = str.slice(0, i);
        let last = str.slice(i+1);

        console.log(`first:${first} last:${last}`);
        
        // 递归调用自身，拼接对调字符
        return acc.concat(anagrams(first+last).map(val => letter + val))

    }, []);

};

anagrams('abc')
// ['abc','acb','bac','bca','cab','cba']
```
思考:如果是中文字符怎么修改代码？

## 参考:
- [30 seconds of code](https://30secondsofcode.org/)
- [48 个 JavaScript 代码片段](https://mp.weixin.qq.com/s/RGSA0VvRcCrQwicVvL9-Xw)
