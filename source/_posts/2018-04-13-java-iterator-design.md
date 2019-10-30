---
title: Java 迭代器(Iterator)设计模式
date: 2018-04-13 09:49:25
categories: Java
---

## 一、定义
迭代器模式（Iterator），提供一种方法顺序访问一个聚合对象中的各种元素，而又不暴露该对象的内部表示。

## 二、角色构成
![](http://images.jessechiu.com/iterator.png)

- 抽象迭代器角色(Iterator): 定义遍历元素所需要的方法，一般来说会有这么三个方法：取得下一个元素的方法 next()，判断是否遍历结束的方法 hasNext()，移出当前对象的方法 remove(),

- 具体迭代器角色(Concrete Iterator)：实现迭代器接口中定义的方法，完成集合的迭代。

- 抽象容器角色(Aggregate):  一般是一个接口，提供一个 iterator() 方法，例如 java 中的 Collection 接口，List 接口，Set 接口等

- 具体容器角色(ConcreteAggregate)：就是抽象容器的具体实现类，比如 List 接口的有序列表实现 ArrayList，List 接口的链表实现 LinkList，Set 接口的哈希列表的实现 HashSet 等。

## 三、使用场景
- 访问一个聚合对象的内容而无需暴露它的内部表示
- 支持对聚合对象的多种遍历
- 为遍历不同的聚合结构提供一个统一的接口

## 四、具体实现代码
```java
public class Main {
    public static void main(String[] args) {
        List list = new ConcreteAggregate();
        list.add("a");
        list.add("b");
        list.add("c");
        list.add("d");
        Iterator it = list.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}
// 1. 定义迭代器角色(Iterator)
interface Iterator {
     boolean hasNext();
     Object next();
}
// 2. 定义具体迭代器角色(Concrete Iterator)
class ConcreteIterator implements Iterator {
    private List list = null;
    private int index;
    public ConcreteIterator(List list) {
        super();
        this.list = list;
    }
    @Override
    public boolean hasNext() {
        if (index >= list.getSize()) {
            return false;
        } else {
            return true;
        }
    }
    @Override
    public Object next() {
        Object object = list.get(index);
        index++;
        return object;
    }
}
// 定义容器角色(Aggregate)
interface List {
    void add(Object obj);
    Object get(int index);
    Iterator iterator();
    int getSize();
}
// 定义具体容器角色(ConcreteAggregate)
class ConcreteAggregate implements List {
    private Object[] list;
    private int size = 0;
    private int index = 0;
    public ConcreteAggregate() {
        index = 0;
        size = 0;
        list = new Object[100];
    }
    @Override
    public void add(Object obj) {
        list[index++] = obj;
        size++;
    }
    @Override
    public Iterator iterator() {
        return new ConcreteIterator(this);
    }
    @Override
    public Object get(int index) {
        return list[index];
    }
    @Override
    public int getSize() {
        return size;
    }
}
```

## 参考
- [Java设计模式系列之迭代器模式](https://www.cnblogs.com/ysw-go/p/5384516.html)