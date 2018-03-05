---
title: Python Start
date: 2017-12-21 14:34:39
updated: 2017-12-21 14:34:39
categories: Vue
---

## 一、基础语法
- 注释：`#`
- 有序集合 [] 类似 js 中的 数组,操作：`push()`,`pop(x)`
- `tuple ()` 也是有序列表，但是创建完后不能修改值
- 使用缩进来区分代码块(使用 4 个空格)
```python
age = 20
if age >= 18:
    print 'your age is', age
    print 'adult'
print 'END'
```
- `dict` 类似 js 中的对象
- `set` 类似 es6 中的 `set`,操作:`add()`,`remove()`
- `def` 定义函数,还可以定义默认参数，规则和 c++ 一样
```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x

// 定义可变参数
def average(*args):
    ...
// 这样，在调用的时候，可以这样写：
average()
0
average(1, 2)
1.5
average(1, 2, 2, 3, 4)
2.4
```
- list 切片
```python
L = ['Adam', 'Lisa', 'Bart', 'Paul']
L[0:3]
['Adam', 'Lisa', 'Bart']
//L[0:3]表示，从索引0开始取，直到索引3为止，但不包括索引3。即索引0，1，2，正好是3个元素。

//如果第一个索引是0，还可以省略：

L[:3]
['Adam', 'Lisa', 'Bart']
//也可以从索引1开始，取出2个元素出来：

L[1:3]
['Lisa', 'Bart']
//只用一个 : ，表示从头到尾：

L[:]
['Adam', 'Lisa', 'Bart', 'Paul']
//因此，L[:]实际上复制出了一个新list。

//切片操作还可以指定第三个参数：
L[::2]
['Adam', 'Bart']
//第三个参数表示每N个取一个，上面的 L[::2] 会每两个元素取出一个来，也就是隔一个取一个。
//把list换成tuple，切片操作完全相同，只是切片的结果也变成了tuple。
```
- 字符串切片
```python
'ABCDEFG'[:3]
'ABC'
'ABCDEFG'[-3:]
'EFG'
'ABCDEFG'[::2]
'ACEG'
```
- 索引迭代 `enumerate()`
```python
L = ['Adam', 'Lisa', 'Bart', 'Paul']
for index, name in enumerate(L):
...     print index, '-', name
... 
0 - Adam
1 - Lisa
2 - Bart
3 - Paul
```
- `dict` 迭代
```python
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
// 使用 values()
print d.values()
# [85, 95, 59]
for v in d.values():
    print v
# 85
# 95
# 59

// 使用 itervalues()
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
print d.itervalues()
# <dictionary-valueiterator object at 0x106adbb50>
for v in d.itervalues():
    print v
# 85
# 95
# 59

// 使用 items() 同事迭代 key 和 value
d = { 'Adam': 95, 'Lisa': 85, 'Bart': 59 }
print d.items()
[('Lisa', 85), ('Adam', 95), ('Bart', 59)]
```
- `strip(rm)`
`s.strip(rm)` 删除 s 字符串中开头、结尾处的 rm 序列的字符。当 rm 为空时，默认删除空白符（包括'\n', '\r', '\t', ' ')，如下：
```python
a = '     123'
a.strip()
// 结果： '123'

a='\t\t123\r\n'
a.strip()
// 结果：'123'
```


## 二、进阶语法
### 1. 函数式编程
- 纯函数式编程: 不需要变量、没有副作用、测试简单
- 支持高阶函数、代码简洁

### 2. 高阶函数
- 能够接收一函数为参数的函数
- `map()` 函数
```python
def f(x):
    return x*x;
result = map(f,[1,2,3]);
for name in enumerate(result):
    print(name)
```

- `reduce()`
```python
def f(x):
    return x+x
reduce(f, [1, 3, 5, 7, 9])
// 带有第 3 个参数
reduce(f, [1, 3, 5, 7, 9], 100)
```

- `filter()`
```python
def is_not_empty(s):
    return s and len(s.strip()) > 0

result = filter(is_not_empty, ['test', None, '', 'str', '  ', 'END'])

for name in enumerate(result):
    print(name)
```

- `sorted()`
 sorted() 函数可对 list 进行排序

```python
sorted([36, 5, 12, 9, 21])
// [5, 9, 12, 21, 36]
// 或者
def reversed_cmp(x, y):
    if x > y:
        return -1
    if x < y:
        return 1
    return 0

sorted([36, 5, 12, 9, 21], reversed_cmp)
// [36, 21, 12, 9, 5]

sorted(['bob', 'about', 'Zoo', 'Credit'])
// ['Credit', 'Zoo', 'about', 'bob']
```

- 匿名函数 `lambda`
关键字 lambda 表示匿名函数，冒号前面的 x 表示函数参数,只能有一个表达式，不写 return，返回值就是该表达式的结果。
```python
map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9])
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

- 装饰器 `decoration` ???
- 中偏函数
`functools.partial`可以把一个参数多的函数变成一个参数少的新函数，少的参数需要在创建时指定默认值，这样，新函数调用的难度就降低了
```python
import functools
int2 = functools.partial(int, base=2)
print(int2('1000000'))
print(int2('1010101'))
```

### 3. 模块和包
- `*.py` -> 模块
- 文件夹 -> 包，在每个文件夹中必须有 `__init__.py` 文件，不然只是普通文件夹
- 引入模块: `import xxx`
- 引入模块其中的几个函数: `from math import sin,pow,log`
- 重命名部分引入的函数名: `from math import sin,pow,log as logger`



## 参考:
- [python 官网](https://www.python.org)
