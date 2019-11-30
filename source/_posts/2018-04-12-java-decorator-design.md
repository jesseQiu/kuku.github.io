---
title: Java 装饰者设计模式
date: 2018-04-12 09:49:25
categories: Java
---

## 一、定义
在不改变原类文件以及不使用继承的情况下，动态地将责任附加到对象上，从而实现动态拓展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。

## 二、设计原则
要使用装饰者模式，需要满足以下设计原则： 
1、多用组合，少用继承 
2、开放/关闭原则：类应该对拓展开放，对修改关闭

## 三、具体步骤
![](https://img-blog.csdn.net/20160803225047686)

1、Component 是基类。通常是一个抽象类或者一个接口，定义了属性或者方法，方法的实现可以由子类实现或者自己实现。通常不会直接使用该类，而是通过继承该类来实现特定的功能，它约束了整个继承树的行为。比如说，如果 Component 代表人，即使通过装饰也不会使人变成别的动物。 

2、Concrete Component 是 Component 的子类，实现了相应的方法，它充当了“被装饰者”的角色。 

3、Decorator 也是 Component 的子类，它是装饰者共同实现的抽象类（也可以是接口）。比如说，Decorator 代表衣服这一类装饰者，那么它的子类应该是T恤、裙子这样的具体的装饰者。 

4、Concrete Decorator 是 Decorator 的子类，是具体的装饰者，由于它同时也是 Component 的子类，因此它能方便地拓展 Component 的状态（比如添加新的方法）。每个装饰者都应该有一个实例变量用以保存某个 Component 
的引用，这也是利用了组合的特性。在持有 Component 的引用后，由于其自身也是 Component 的子类，那么，相当于 Concrete Decorator 包裹了 Component，不但有 Component 的特性，同时自身也可以有别的特性，也就是所谓的装饰。

## 四、代码示例
```java
package com.company;
public class Main {
    public static void main(String[] args) {
        Person person = new Man("Shopping list: ");
        // 经过衬衫装饰
        person = new Shirt(person);
        System.out.println(person.getDescription() + " " + person.cost());
        // 再经过鸭舌帽装饰
        person = new Casquette(person);
        System.out.println(person.getDescription() + " " + person.cost());
    }
}
// 1. 创建 Component 基类
abstract class Person {
    String description;
    public Person() {
    }
    public Person(String des) {
        this.description = des;
    }
    public String getDescription() {
        return description;
    }
    // 子类应该实现的方法
    public abstract double cost();
}
// 2. 创建被装饰者 Concreate Component
class Man extends Person {
    public Man(String des) {
        super(des);
    }
    @Override
    public double cost() {
        // 什么都没买，不用钱
        return 0;
    }
}
// 3. 创建 Decorator 种类
// 衣服装饰者
abstract class ClothingDecorator extends Person {
    public abstract String getDescription();
}
// 帽子装饰者
abstract class HatDecorator extends Person {
    public abstract String getDescription();
}
// 4. 创建 Concreate Decorator (具体种类的装饰者)
// 衬衫装饰者 -> 继承衣服装饰类
class Shirt extends ClothingDecorator {
    // 用实例变量保存 Person 的引用
    Person person;
    public Shirt(Person person) {
        this.person = person;
    }
    @Override
    public String getDescription() {
        return person.getDescription() + "a shirt  ";
    }
    @Override
    public double cost() {
        // 实现了 cost() 方法，并调用了 person 的 cost() 方法，目的是获得所有累加值
        return 100 + person.cost();
    }
}
// 鸭舌帽装饰者 ->　继承帽子装饰类
class Casquette extends HatDecorator {
    Person person;
    public Casquette(Person person) {
        this.person = person;
    }
    @Override
    public String getDescription() {
        // 鸭舌帽
        return person.getDescription() + "a casquette  ";
    }
    @Override
    public double cost() {
        return 75 + person.cost();
    }
}
```

## 五、特点

- 装饰者和被装饰者有相同的接口（或有相同的父类）。 
- 装饰者保存了一个被装饰者的引用。 
- 装饰者接受所有客户端的请求，并且这些请求最终都会返回给被装饰者。 
- 在运行时动态地为对象添加属性，不必改变对象的结构。


## 六、应用场景

Java 中经常出现的地方就是 JavaIO。提到 JavaIO，脑海中就冒出了大量的类：InputStream、FileInputStream、BufferedInputStream……等。其实，这里面大部分都是装饰类，只要弄清楚这一点就容易理解了。

![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/io-class.png)

下面是利用 javaIo 类实现的一个装饰，实现对单词首字母转为大写。
```java
package com.company;

import java.io.*;

public class Main {
    public static void main(String[] args) {
        int c = -1;
        StringBuffer sb = new StringBuffer();
        try {
            // 这里用了两个装饰者，
            // 分别是 BufferedInputStream 和我们的 UpperFirstWordInputStream
            InputStream in = new UpperFirstWordInputStream(new BufferedInputStream(new FileInputStream("d:\\test.txt")));
            while ((c = in.read()) >= 0) {
                sb.append((char) c);
            }
            System.out.println(sb);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
// 对文本中单词首字母转换为大写
class UpperFirstWordInputStream extends FilterInputStream {
    private int cBefore = 32;
    protected UpperFirstWordInputStream(InputStream in) {
        // 由于 FilterInputStream 已经保存了装饰对象的引用，这里直接调用 super 即可
        super(in);
    }
    public int read() throws IOException {
        // 根据前一个字符是否是空格来判断是否要大写
        int c = super.read();
        if (cBefore == 32) {
            cBefore = c;
            return (c == -1 ? c : Character.toUpperCase((char) c));
        } else {
            cBefore = c;
            return c;
        }
    }
}
```

## 参考
- [学习、探究Java设计模式——装饰者模式](https://blog.csdn.net/a553181867/article/details/52108423)