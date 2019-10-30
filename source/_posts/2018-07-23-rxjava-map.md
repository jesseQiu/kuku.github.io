---
title: RxJava Map
date: 2018-07-23 14:34:39
updated: 2018-07-23 14:34:39
categories: Java
---

## 一、变换
### 1. Map
```java
String[] str1 = {"1", "2", "3"};
Observable.fromArray(str1)
        // 这里用了 Function 包装接口，返回一个新的值
        .map(new Function<String, String>() {
            @Override
            public String apply(String s) throws Exception {
                return "apply: " + s;
            }
        })
        .subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                System.out.println("s = [" + s + "]");
            }
        });
// 运行结果
// 运行结果
s = [apply 1]
s = [apply 2]
s = [apply 3]
```

### 2. FlatMap
```java
// 学生类
class Student{
    private String name;
    private String [] course;
    public Student(String name, String[] course) {
        this.name = name;
        this.course = course;
    }
    public String[] getCourse() {
        return course;
    }
    public String getName() {
        return name;
    }
}
String[] course1 = {"a","b","c"};
String[] course2 = {"d","e","f"};
Student[] students = {new Student("kuku",course1),new Student("lili",course2)};
// 这里通过 flatMap 获取每个学生的课程
Observable.fromArray(students)
        .flatMap(new Function<Student, ObservableSource<String>>() {
            // 多了一层 Observable 来处理
            @Override
            public ObservableSource<String> apply(Student student) throws Exception {
                return Observable.fromArray(student.getCourse());
            }
        })
        .subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                System.out.println("s = [" + s + "]");
            }
        });
// 运行结果
s = [a]
s = [b]
s = [c]
s = [d]
s = [e]
s = [f]
```


## 参考链接
- [给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)

