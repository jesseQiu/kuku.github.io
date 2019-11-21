---
title: Java Grammar
date: 2018-04-09 09:49:25
categories: Java
---

本文主要记录了学习过程中的语法知识笔记，并不系统，主要是针对个人比较容易混淆或难以理解的点记录，方便后期可以通过本文快速回忆


## 一、基础

### 1.1 java 包
- 管理 java 文件
- 解决 **同名文件** 冲突
- 包名称全部使用小写，用 `.` 号连接
- 包名一般使用 **公司网址反写+项目名称**: `com.jesse.myjava`

### 1.2 八种数据类型
![](http://images.jessechiu.com/8-data-type.png)

- 方法定义修饰符顺序
访问权限修饰符 + 静态修饰符 + 返回值类型 + 方法名称
```java
public static void main(String[] args){
	// ...
}
```

### 1.3 变量初始化
成员变量: 有默认初始值，引用类型默认为 null；
局部变量: 没有默认初始值，必须定义赋值后才能使用;
```java
class Cars{
    private int money; // 成员变量
    public Cars(){
        int heavy;// 局部变量
        System.out.println(money);// 0
        System.out.println(heavy);// 错误
    }
}
Cars car = new Cars();
```

### 1.4 包与访问修饰符
- `package.com.xxx` 语句只能出现在代码中第一句
- 访问修饰符
![](http://images.jessechiu.com/package.png)



## 二、面向对象
### 2.1 对象一对一关系(可以双向也可以单向)
```java
public class Main {
    public static void main(String[] args) {
        Hearo hearo = new Hearo("jesse");
        Weapon weapon = new Weapon(300);

        // 把两个对象关联起来
        hearo.setWeapon(weapon);
        weapon.setHearo(hearo);

        // 通过各自实例获取对方信息
        System.out.println("hearo: " + hearo.getName() + " fire: " + hearo.getWeapon().getFire());
        System.out.println("hearo: " + weapon.getHearo().getName() + " fire: " + weapon.getFire());
    }
}
class Hearo{
    private String name;
    private Weapon weapon; // 一对一关系
    public Hearo(){};
    public String getName() {
        return name;
    }
    public Hearo(String name){
        this.name = name;
    }
    public void setWeapon(Weapon weapon) {
        this.weapon = weapon;
    }
    public Weapon getWeapon() {
        return weapon;
    }
}
class Weapon {
    private int fire;
    private Hearo hearo;// 双向一对一关系
    public Weapon(){};
    public int getFire() {
        return fire;
    }
    public Weapon(int fire){
        this.fire = fire;
    }
    public void setHearo(Hearo hearo) {
        this.hearo = hearo;
    }
    public Hearo getHearo() {
        return hearo;
    }
}
```

### 2.2 静态变量、方法
- 静态属性和方法是全局，生命周期长
- 静态函数只能访问静态属性和方法
- 静态初始块只能初始静态变量
- 普通函数只能通过 **类** 访问静态函数


### 2.3 代码块
- 普通代码块，在方法中写的代码
- 构造代码块，在类中定义的代码块，创建对象时被调用，由构造方法执行
- 静态代码块，在类中使用 `static` 声明的代码块,创建对象时执行，只执行一次(全局)，通常用于在项目中初始化时，初始数据用。
- 同步代码块(多线程中使用)
```java
public class Main {
    public static void main(String[] args) {
        Hearo hearo1 = new Hearo();
        Hearo hearo2 = new Hearo();
        hearo1.getName();
        hearo2.getName();
    }
}
class Hearo{
    private String name;

    public Hearo(){
        System.out.println("构造方法!");
    };
    {
        System.out.println("我是构造代码块！");
    }
    static {
        System.out.println("*** 我是静态代码块！");
    }
    public String getName() {
        {
            System.out.println("我是普通代码块!");
        }
        return name;
    }
}
// 执行结果
***我是静态代码块！
我是构造代码块！
构造方法!
我是构造代码块！
构造方法!
我是普通代码块!
我是普通代码块!
```
**执行顺序: 静态代码块 -> 构造代码块 -> 构造方法**


### 2.4 单列模式
- 定义: 保证一个类只有一个实例，并提供一个访问它的全局访问点
- 应用场景和优点：主要应用在工具类，只有功能方法没有属性，且需要频繁调用。避免频繁创建对象产生内存消耗。
```java
public class Main {
    public static void main(String[] args) {
        Hearo hearo = Hearo.getInstance();
        hearo.test();
    }
}
class Hearo{
    // 构造方法私有化
    private Hearo(){};
    // 声明一个本类对象
    private static Hearo hearo = new Hearo();
    // 提供外部获取实例静态方法
    public static Hearo getInstance() {
        return hearo;
    }
    public void test(){
        System.out.println("测试方法");
    }
}
```

### 2.5 继承
- 子类实例化时，为什么要调用父类的构造函数? 子类需要调用父类的属性值，而父类的属性值必须通过构造函数初始化。
- protected 修饰的属性方法(受保护的访问权限修饰符)，可以被子类继承
- 父类中被 `private,static,final` 中任意修饰符修饰，则不能被子类重写
- 当父类没有无参数构造函数且有其它构造函数时，子类需要显示调用父类有参构造函数，使用 `super()` 方法调用
```java
public class Main {
    public static void main(String[] args) {
        Hearo hearo1 = new Hearo();
        Hearo hearo2 = new Hearo("jesse");
        hearo1.test();
    }
}
class People{
    protected int age = 18;
    private String name;
    public People(String name){
        System.out.println("People 有参构造函数");
        this.name = name;
    }
    public void test(){
        System.out.println("People test()");
    }
}
class Hearo extends People{
    public Hearo(){
        super("lili");
        System.out.println("Hearo 无参构造函数");
    }
    public Hearo(String name){
        super(name);
        System.out.println("Hearo 有参构造函数");
    }
    public void test(){
        // 调用父类方法
        super.test();
        // 调用父类 age 属性,父类 age 属性必须是 public 或 protected
        System.out.println(super.age);
        System.out.println("Hearo test()" );
    }
}
// 调用结果
Hearo 无参构造函数
People 有参构造函数
Hearo 有参构造函数
People test()
18
Hearo test()
```

### 2.6 `final` 关键字
- 使用 `final` 声明一个常量(最终变量): 修饰属性或者局部变量，建议使用大写。
- 使用 `final` 声明一个方法: 该方法是最终方法，只能被子类继承，不能被重写(不常有)。
- 使用 `final` 声明一个类: 该类为最终类，不能被继承(工具类)。
- 在方法参数中使用: 在该方法内部不能修改该参数值。
```java
public class Main {
    public static void main(String[] args) {
        People people = new People("jese");
        people.sayHello(2);
    }
}
// 定义了 final 类，不能被继承
final class People{
    // 定义时初始化
    public final int AGE = 18;
    public final String NAME;
    public People(String name){
        // 在构造函数中初始化
        this.NAME = name;
    }
    // 定义 final 方法，不能被重写
    final public void sayHello(final int times){
        // times = 3;// 这里的 times 是被 final 修饰，不能修改
        System.out.println("name: "+ this.NAME + " age: " + this.AGE);
    }
}
```

### 2.7 抽象类
- 很多具有相同 **行为** 和 **特征** 的类，可以抽象为一个抽象类，使用 `abstract` 关键字修饰 
```java
public class Main {
    public static void main(String[] args) {
        Man man = new Man();
        man.move();
        man.eat();
        man.sleep();

        Women women = new Women();
        women.move();
        women.eat();
        women.sleep();

        // 抽象类不能实例化
        // Animal animal = new Animal();
        // Peole peole = new Peole();
    }
}

// 定义一个抽象类
abstract class Animal {
    // 抽象类方法声明，注意: 只有声明没有实现,不能有 {}
    public abstract void move();
}

// 抽象类可以继承于抽象类
abstract class Peole extends Animal {
    public abstract void eat();
    // 抽象类可以有自己的实现方法
    public void sleep() {
        System.out.println("睡觉");
    }
}

// 继承抽象类的具体类，必须实现所有抽象类
class Man extends Peole {
    public void move() {
        System.out.println("我是 man 在移动");
    }
    public void eat() {
        System.out.println("我是 man 在吃东西");
    }
}
class Women extends Peole {
    public void move() {
        System.out.println("我是 women 在移动");
    }
    public void eat() {
        System.out.println("我是 women 在吃东西");
    }
}
```

### 2.8 接口
- 定义格式
```java
interface 接口名称{
    全局常量;
    抽象方法;
}
```
- 接口不是类，是一组行为的规范、定义、没有实现的方法(JDK 1.8支持)
- 接口可以继承于接口，且可以多继承(类只能单继承)
- 一个具体实现接口使用 `implements` 关键字
- 抽象类实现接口可以不实现接口方法
- 接口不能被实例化
```java
public class Main {
    public static void main(String[] args) {
        Girl girl = new Girl("lili");
        girl.eat();
        girl.run();
        girl.run();
    }
}
// 定义一个接口
interface IEat{
    // public abstract void eat(); // 接口中只能定义抽象方法
    // 接口中定义方法没有声明修饰符，默认为 public abstract,所以等价于下面
    void eat();
    // public final int NUM = 10;
    int NUM = 10;

    // 1.8 JDK 新特性，可以被所有实现类继承
//    public default void print(){
//        System.out.println("IEeat print()");
//    }
}
interface IRun{
    void run();
}
// 接口之间可以多继承(注意: 类只能单继承)
interface ISleep extends IEat,IRun{
    void sleep();
}
// 具体类实现接口必须实现接口的所有方法，这里还可以多重实现不同接口
class Girl implements ISleep,IEat{
    private String name;
    public Girl(String name){
        this.name = name;
    }
    public Girl(){};
    public void sleep(){
        System.out.println("我爱睡觉");
    }
    public void eat(){
        System.out.println("我爱吃");
    }
    public void run(){
        System.out.println("我爱跑步");
    }
}
```

### 2.9 多态性
- 定义: 对象在运行时的多种形态，分为方法多态和对象多态
    * 方法多态: 重写和重载就是方法的多态表现
    * 对象多态: 用父类的引用指向子类对象(用大的类型指向小的类型,向上转型,自动转换)
- 实际开发中尽量使用父类引用(便于扩展)
- 多个子类就是父类中的多种多态
```java
public class Main {
    public static void main(String[] args) {
        // 用父类的引用指向子类对象(用大的类型指向小的类型,向上转型,自动转换)
        // 多个子类就是父类(Chicken)中的多种多态
        Chicken hc = new HomeChicken("家鸡");
        hc.eat();
        Chicken wc = new Wildhicken("野鸡");
        wc.eat();

        // 也可以通过参数传递，实现多态
//        eat(hc);
//        eat(wc);
        Chicken sc = new SongChicken("唱歌鸡");
        eat(sc);
    }
    public static void eat(Chicken c){
        System.out.println("鸡吃饭");
        c.eat();
        // 子类引用指向父类需要强制转换
        // 避免转换异常，添加一个判断
        if(c instanceof SongChicken){
            SongChicken songChicken = (SongChicken)c;
            ((SongChicken) c).song();
        }
    }
}
// 定义抽象类 鸡对象
abstract class Chicken {
    private String name;
    public Chicken(){}
    public Chicken(String name){
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    // 定义抽象类
    public abstract void eat();
}
// 具体类实现 家鸡
class HomeChicken extends Chicken{
    public HomeChicken(String name){
        super(name);
    }
    public void eat(){
        System.out.println(this.getName() + " 爱吃米");
    }
}
// 具体类实现 野鸡
class Wildhicken extends Chicken{
    public Wildhicken(String name){
        super(name);
    }
    public void eat(){
        System.out.println(this.getName() + " 爱吃虫子");
    }
}
// 具体类实现 唱歌鸡
class SongChicken extends Chicken{
    public SongChicken(String name){
        super(name);
    }
    public void eat(){
        System.out.println(this.getName() + " 爱唱歌");
    }
    public void song(){
        System.out.println("我在唱歌!");
    }
}
// 运行结果
家鸡 爱吃米
野鸡 爱吃虫子
鸡吃饭
唱歌鸡 爱唱歌
我在唱歌!
```

### 2.10 内部类
1. 内部类提供了更好的封装，可以把内部类隐藏在外部类之内，不允许同一个包中的其他类访问该类
2. 内部类的方法可以直接访问外部类的所有数据，包括私有的数据
3. 内部类所实现的功能使用外部类同样可以实现，只是有时使用内部类更方便
4. 通常建议使用静态内部类(不会产生内存泄漏)
5. 内部类有效的实现了 "多重继承"(内部类也会被子类继承)
```java
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        // 1. 调用成员内部类
        // 在外部创建成员内部类，通常不建议这么使用
        Outer.Inner inner = outer.new Inner();
        // 调用成员内部类方法
        inner.print();
        // 建议使用下面方法调用成员内部类方法
        outer.innerPrint();

        // 2. 调用方法内部类
        outer.show();

        // 3. 调用静态内部类
        // 静态内部类不需要依赖外部类实例也可以访问
        Outer.Inner3 inner3 = new Outer.Inner3();
        inner3.print();

        // 4. 调用匿名内部类
        outer.print1();
        outer.print2();
        outer.print3(new Eat(){
            public void eat(){
                System.out.println("参数式 匿名内部类");
            }
        });
    }
}
// 定义外部类
class Outer{
    private String name;
    // 对外提供访问内部类方法，建议使用
    public void innerPrint(){
        Inner inner = new Inner();
        inner.print();
    }
    // 1. 成员内部类
    class Inner{
        public void print(){
            System.out.println("成员内部类");
        }
    }
    // 2. 方法内部类
    public void show(){
        final int num = 18;
        int age = 8;
        // 定义方法内部类
        class Inner2{
            public void print(){
                // 内部类使用方法中的变量必须是 final 变量,age 不是 final 变量，所以不能使用
                System.out.println("方法内部类: " + num);
            }
        }
        // 只能在定义方法内部实例化，不可以在此方法外部实例化
        Inner2 inner2 = new Inner2();
        inner2.print();
    }
    // 3. 静态内部类
    // 只能访问外部类的静态 成员 和 方法
    static class Inner3{
        public void print(){
            System.out.println("静态内部类");
        }
    }
    // 4. 匿名内部类
    // - 不能有构造方法，只能有一个实例
    // - 不能定义任何静态成员和方法
    // - 不能是 public protected private static
    // - 一定是在 new 后面，用其隐含实现一个接口或实现一个类
    // - 匿名内部类属于局部类，所以局部类的所有限制都对其生效

    // 继承式 匿名内部类
    public void print1(){
        // 定义匿名内部类
        Cat cat = new Cat(){
            public void eat(){
                System.out.println("继承式 匿名内部类");
            }
        };
        cat.eat();
    }
    // 接口式 匿名内部类
    public void print2(){
        // 定义匿名内部类
        Eat eat = new Eat(){
            public void eat(){
                System.out.println("接口式 匿名内部类");
            }
        };
        eat.eat();
    }
    // 参数式 匿名内部类
    public void print3(Eat eat){
        eat.eat();
    }

}
// 定义抽象类
abstract class Cat{
    public abstract void eat();
}
// 定义接口
interface Eat{
    void eat();
}
// 运行结果
成员内部类
成员内部类
方法内部类: 18
静态内部类
继承式 匿名内部类
接口式 匿名内部类
参数式 匿名内部类
```

## 三、Lambada 表达式(JDK 1.8)
Lambada 表达式(也称闭包)允许把函数作为一个方法的参数，或者把代码看作数据，用于简化接口式的匿名内部类。
```java
public class Main {
    public static void main(String[] args) {
        // 1. 使用实现类
        IEat iEat1 = new IEatPlug();
        iEat1.eat();

        // 2. 使用匿名类
        IEat iEat2 = new IEat() {
            @Override
            public void eat() {
                System.out.println("eat 接口匿名类");
            }
        };
        iEat2.eat();

        // 3. Lambada 方式
        // 前提条件，接口只能有一个接口函数，可以有 default 和 static 函数
        // 好处: 代码简洁、不会单独生成 class 文件
        // 类似 javascript es6 的箭头函数用法
//        IEat iEat3 = ()->{System.out.println("eat Lambada");};
        IEat iEat3 = ()->System.out.println("eat Lambada");
        iEat3.eat();
    }
}
interface IEat{
    void eat();
}
class IEatPlug implements IEat{
    @Override
    public void eat() {
        System.out.println("eat 接口实现类");
    }
}
```

## 四、文件 与 IO
- File 类表示一个文件 或 目录
- 流的类  别
    * 字节流: 一般在操作非字符文件时使用字节流
    * 字符流: 操作字符文件时使用，内部也是使用字节流操作

## 五、字符编码
- iso8859-1: 单字节编码，最多只能表示 0-255 之间的字符，主要应用在英文
- GBK/2312: 双字节编码，中文的国际编码，专门用来表示汉字
- unicode: 双字节编码，使用 16 进制表示编码，java 中使用此编码方式，也是最标准的一种编码。但是不兼容 iso8859-1 编码。
- utf: 1-6 字节不定长度编码。由于 uniconde 编码不支持 iso8859-1 编码，在表示英文字母时也使用 2 个字节，占用空间，不便于传输和存储，因此产生了 uft 编码。
- 造成乱码根本原因:
    * 程序使用的编码与本机的编码不统一
    * 在网络中，客服端与服务端编码不统一
```java
public class Main {
    public static void main(String[] args) {
        String info = "我在唱歌，啦啦啦啦...";

        try {
            // 以 utf-8 编码格式读取，转为 iso8859-1 编码格式的 字符串
            String  str1 = new String(info.getBytes("utf-8"),"iso8859-1");
            System.out.println("str1: " + str1);

            // 上面的逆操作
            String  str2 = new String(str1.getBytes("iso8859-1"),"utf-8");
            System.out.println("str2: " + str2);
            
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
    }
}
//运行结果
str1: æå¨å±æ­ï¼å¦å¦å¦å¦...
str2: 我在唱歌，啦啦啦啦...
```

## 六、Java NIO(New IO)
Java NIO（New IO）是从 Java 1.4 版本开始引入的一个新的 IO API，可以替代标准的 Java IO AP.

### 6.1 Java NIO 提供了与标准 IO 不同的 IO 工作方式： 
- Channels and Buffers（通道和缓冲区）：标准的 IO 基于字节流和字符流进行操作的，而NIO是基于通道（Channel）和缓冲区（Buffer）进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。 

- Asynchronous IO（异步IO）：Java NIO 可以让你异步的使用IO，例如：当线程从通道读取数据到缓冲区时，线程还是可以进行其他事情。当数据被写入到缓冲区时，线程可以继续处理它。从缓冲区写入通道也类似。 

- Selectors（选择器）：Java NIO 引入了选择器的概念，选择器用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个的线程可以监听多个数据通道。

下面是一个利用 channel 和 buffers 读取文件，并打印显示
```java
public class Main {
    public static void main(String[] args) {
        RandomAccessFile aFile = null;
        try {
            // 通过 RandomAccessFile 来获取一个 FileChannel 实例
            aFile = new RandomAccessFile("D:\\test.txt", "rw");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        // 获取 channel
        FileChannel inChannel = aFile.getChannel();
        // create buffer with capacity of 48 bytes
        ByteBuffer buf = ByteBuffer.allocate(48);
        // read into buffer.
        int bytesRead = 0;
        try {
            // 从 channel 读取数据
            bytesRead = inChannel.read(buf);
        } catch (IOException e) {
            e.printStackTrace();
        }
        while (bytesRead != -1) {
            // make buffer ready for read
            // 反转，让 buf 从写状态转为读状态，指针指向 0 位置
            buf.flip();
            while (buf.hasRemaining()) {
                // read 1 byte at a time
                System.out.print((char) buf.get());
            }
            // make buffer ready for writing
            buf.clear();
            try {
                bytesRead = inChannel.read(buf);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        try {
            aFile.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

## 七、`Path` 和 `Files`

文件或是目录习惯用 java.io.File 对象来表示，但是 File 类有很多缺陷，比如它的很多方法不能抛出异常，它的 delete 方法经常莫名其妙的失败等，旧的 File 类经常是程序失败的根源。因此在 Java7 中有了更好的替代：java.nio.file.Path 及 java.nio.file.Files

- Path 可以是一个文件，一个目录，或是一个符号链接，也可以是一个根目录。用法很简单。创建 Path 并不会创建物理文件或是目录，path 实例经常引用并不存在的物理对象，要真正创建文件或是目录，需要用到 Files 类。

- Files 提供了处理文件和目录以及读取文件和写入文件的静态方法。可以用它创建和删除路径。复制文件。检查路径是否存在等。此外。Files 还拥有创建流对象的方法。

- 基础用法:
```java
// 并没有实际创建路径，而是一个指向 d:/users/test.txt 路径的引用
Path path = FileSystems.getDefault().getPath("d:/users/test.txt");
Path path = Paths.get("d:/users/test.txt");      
path.toString();          //得到全路径---d:/users/日记5.txt
path.getFileName(); //得到文件名---日记5.txt
path.getParent();       //得到父目录---d:/users
path.getNameCount();   //得到目录中元素的个数，不算根---2
path.getname(0);    //得到路径中第一个元素名，不算根---users
path.getname(1);    //得到路径中第二个元素名，不算根---日记5.txt
path.getRoot();        //得到根目录---/

Path pathfile = Paths.get("d:/users/日记5.txt");
Path  pathdirec = Paths.get("d:/users");
Files.createFile(pathfile);                //创建文件
Files.createDirectory(pathdirec);   //创建目录
Files.delete(pathfile);                      //直接删除路径
Files.deleteIfExists(pathfile);         //先判断是否存在，存在再删
```

- 综合实例
```java
public class Main {
    public static void main(String[] args) {
        // 复制文件
        Path source = Paths.get("d:\\test.txt");
        Path target = Paths.get("d:\\test-copy.txt");
        // 先判断是否存在该文件，如果没有者创建
        if(!Files.exists(source)){
            try {
                // 创建文件
                Files.createFile(source);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        try {
            // 文件拷贝
            Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        // 文件移动
        // 移动的目标路径必须是存在的
        Path path = Paths.get("d:\\test");
        if(!Files.exists(path)){
            try {
                // 创建路径
                Files.createDirectory(path);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        // 拼接完整的路径
        Path target2 = Paths.get(path.toString(),"test-move.txt");
        try {
            Files.move(source, target2, StandardCopyOption.REPLACE_EXISTING);
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        // 读出和写入操作
        Path textFile = Paths.get("d:\\write.txt");
        String line1 = "你好：";
        String line2 = "Files";
        List<String> lines = Arrays.asList(line1, line2);
        Charset charset = Charset.forName("UTF-8");
        try {
            Files.write(textFile, lines, charset);
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        List<String> linesRead = null;
        try {
            // 读取文件
            linesRead = Files.readAllLines(textFile, charset);
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        if (linesRead != null) {
            for (String line : linesRead) {
                System.out.println(line);
            }
        }
    }
}
```

## 八、集合

### 8.1 集合类总图
![](http://images.jessechiu.com/collection-class.jpg)

### 8.2 Collection 接口
用于存储单个对象的集合

#### 8.2.1 List 接口特点:
- 有序
- 允许多个 null 元素,可以重复
- 具体实现主要有以下 3 个:
    - ArrayList
        * 实现原理: 采用动态对象数组实现，默认构造方法创建一个空数组
        * 扩充方法: 第一次添加元素，扩展量为 10 个，之后的扩充增量是原来大小的一半
        * 不适合删除或插入(因为本质是数组，需要移动，效率低)
        * 为了防止数组动态扩充次数过多，建议创建 ArrayList 时，给定初始容量
        * 多线程使用 `不安全`，适合在 单 线程使用，在单线程下使用效率 高 

    - Vector
        * 实现原理: 采用动态对象数组实现，默认构造方法创建了一个大小为 10 的对象数组
        * 扩充方法: 当增量为 0 时，扩充为原来大小的 2 倍，当增量大于 0 时，扩充为原来大小 + 增量
        * 不适合删除或插入(因为本质是数组，需要移动，效率低)
        * 为了防止数组动态扩充次数过多，建议创建 Vector 时，给定初始容量
        * `线程安全`，适合在多线程访问时使用，在单线程下使用效率 低

    - LinkedList
        * 实现原理: 采用双向链表结构
        * 适合插入删除操作，性能高


#### 8.2.2 Set 接口特点:
- 无序
- 不允许重复元素
- 具体实现主要有以下 3 个:
    - HashSet
        1. 实现原理: 基于哈希表(HashMap)实现
        2. `不允许重复`，可以有一个 Null 元素
        3. 不保证顺序永久不变
        4. 添加元素时把元素作为 HashMap 的 key 存储，HashMap 的 value 使用一个固定的 object 对象
        5. 排除重复元素是通过 equals 来检查对象是否相同
        6. 判断两个对象是否相同，先判断对象的 hanshCode 是否相同，不同:不是同一个对象，相同:再用 equals 判断。
        7. 如果要让自定义对象的属性值都相同判断为同一个对象，需要重写 hashCode 和 equals 方法 
        8. 存储结构: 数组+链表，数组里的元素以链表的形式存储
    
    - TreeSet
        - 有序，基于 TreeMap(二叉树数据结构)，对象需要比较大小，通过对象比较器来实现，对象比较器还可以用来去除重复元素。如果自定义的数据类，没有实现比较器接口，将无法添加到 TreeSet 集合中。

    - LinkedHashSet
        - 无序，但是保证添加顺序


#### 8.2.3 Map 接口特点:
- 无序
- 使用键值对存储
- 具体实现主要有以下几个
    - HashMap
        - 扩充原理: 当数组的容量超过 75%,数组扩充为原来的 1 倍
        - 基于哈希表(数组 + 链表 + 二叉树(红黑树)) 1.8 JDK
        - 默认加载因子 0.75，默认数组大小 16
        - `线程不安全`

    - HashTable
        - JDK 1.0
        - 基于哈希表(数组+链表)
        - 默认数组大小为 11，加载因子 0.75
        - 扩充方式: 原来数组的一倍 + 1
        - `线程安全`

    - LinkedHashMap
        - 由于 HashMap 不能保证顺序恒久不变，此类添加了双重链表来维护元素的添加顺序

    - TreeMap


### 8.3 集合 Iterator 接口
```java
public class Main {
    public static void main(String[] args) {
        Vector<String> llt = new Vector<>();
        llt.add("a");
        llt.add("b");
        llt.add("c");
        llt.add("a");

        // JDK 1.5 后部署 - 推荐使用
        for(String s:llt){
            System.out.println(s);
        }
        // 效率最高 - 推荐使用
        Iterator<String> it = llt.iterator();
        while(it.hasNext()){
            System.out.println(it.next());
        }
        // elements 方法并非所有集合都有
        Enumeration<String> es = llt.elements();
        while(es.hasMoreElements()){
            System.out.println(es.nextElement());
        }
        // JDK 1.8 后部署
        llt.forEach((String s) -> System.out.println(s));
        llt.forEach( s -> System.out.println(s));
        llt.forEach(System.out::println);
    }
}
```

## 九、四大核心函数式接口(JDK 1.8)
```java
import java.util.*;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        LambdaFunction lambdaFunction = new LambdaFunction();
        lambdaFunction.run();
    }
    // 测试四大函数式接口类
    public static class LambdaFunction{
        public void run(){
            test1();
            test2();
            test3();
            test4();
        }

        // Consumer<T> : 消费型接口
        public void test1() {
            happy(50000.0, (money)->System.out.println("消费型接口: 我爱吃鱼，每次消费"+money+"元")) ;
        }
        public void happy(Double money,Consumer<Double> consumer){
            consumer.accept(money);
        }

        // Supplier<T> : 供给型接口
        public void test2(){
            List<Integer> list = supply(10,()->(int)(Math.random()*100))  ;
            System.out.println("供给型接口: ");
            list.forEach(System.out::println);
        }

        // 随机生成 指定数量的 0-100 随机数
        public List<Integer> supply(Integer num ,Supplier<Integer> supplier){
            List<Integer> resultList=new ArrayList<Integer>()   ;
            for(int x=0;x<num;x++)
                resultList.add(supplier.get())   ;
            return resultList ;
        }

        // Function<T, R> : 函数型接口
        public void test3(){
            String string = handleStr("\t\t\t\t 看电影！！！   ",(str)->str.trim())    ;
            System.out.println("函数型接口: " + string)   ;
            String string2 = handleStr("我爱游泳哒哒哒", str->str.substring(2, 6));
            System.out.println("函数型接口: " + string2);
        }
        // 去除输入字符的头尾空格
        public String handleStr(String target,Function<String, String> fun){
            return fun.apply(target)   ;
        }

        // Predicate<T> : 断言型接口
        public void test4(){
            List<String>  list=Arrays.asList("atguigu","mldn","bjpowernode","itcast","sxt")   ;
            List<String> newList = filterStr(list,string->string.length()>3) ;
            System.out.println("断言型接口:");
            newList.stream().forEach(System.out::println) ;
        }
        // 提取字符数 >3 的元素
        public List<String> filterStr(List<String> list,Predicate<String> predicate){
            List<String>  resultList=new ArrayList<>()   ;
            int size = list.size();
            for(int x=0;x<size;x++){
                String string = list.get(x) ;
                if(predicate.test(string))
                    resultList.add(string)    ;
            }
            return resultList  ;
        }
    }
}
```

## 十、Stream
![](http://images.jessechiu.com/stream.png)

Java 8 中的 Stream 是对集合（Collection）对象功能的增强，它专注于对集合对象进行各种非常便利、高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。Stream API 借助于同样新出现的 Lambda 表达式，极大的提高编程效率和程序可读性。同时它提供串行和并行两种模式进行汇聚操作，并发模式能够充分利用多核处理器的优势，使用 fork/join 并行方式来拆分任务和加速处理过程。通常编写并行代码很难而且容易出错, 但使用 Stream API 无需编写一行多线程的代码，就可以很方便地写出高性能的并发程序。所以说，Java 8 中首次出现的 java.util.stream 是一个函数式语言+多核时代综合影响的产物。
下面是一个综合实例对比
```java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Stone> stoneList = new ArrayList<>();
        // 初始化数据
        stoneList.add(new Stone("a",22));
        stoneList.add(new Stone("q",200));
        stoneList.add(new Stone("n",1002));
        stoneList.add(new Stone("v",3000));
        stoneList.add(new Stone("j",600));
        stoneList.add(new Stone("o",49));
        System.out.println("处理前: ");
        stoneList.forEach(s->System.out.println(s.getWeight()));

        // 过虑 -> 排序 -> 提取名称

        // ---------  jdk 1.7 实现  ----------
        // 过虑
        List<Stone> stoneList1 = new ArrayList<>();
        for(Stone s:stoneList){
            if(s.getWeight() > 300){
                stoneList1.add(s);
            }
        }
        System.out.println("过虑 < 300: ");
        stoneList1.forEach(s->System.out.println(s.getWeight()));
        // 排序(使用了匿名类)
        // Collections 工具类方法
        Collections.sort(stoneList1, new Comparator<Stone>() {
            @Override
            public int compare(Stone o1, Stone o2) {
                return Integer.compare(o1.getWeight(),o2.getWeight());
            }
        });
        System.out.println("按重量排序: ");
        stoneList1.forEach(s->System.out.println(s.getWeight()));
        // 提取名称
        List<String> stoneName = new ArrayList<>();
        for(Stone s:stoneList1){
            if(s.getWeight() > 300){
                stoneName.add(s.getName());
            }
        }
        System.out.println("提取名字: ");
        stoneName.forEach(s->System.out.println(s));

        // --------- jdk 1.8 ------------
        List<String> stoneName1 = stoneList.stream()
                .filter(s -> s.getWeight() > 300)
                .sorted(Comparator.comparing(s->s.getWeight()))
                .map(s->s.getName())
                .collect(Collectors.toList());
        System.out.println("JDK-1.8 过虑 -> 排序 -> 提取名称: ");
        stoneName1.forEach(s->System.out.println(s));
    }
}
class Stone{
    private int weight;
    private String name;
    public Stone(String name,int weight) {
        this.weight = weight;
        this.name = name;
    }
    public String getName() {
        return name;
    }
    public int getWeight() {
        return weight;
    }
    public void setWeight(int weight) {
        this.weight = weight;
    }
}
```

## 十一、`Optional` 容器类
调用一个方法得到了返回值，我们首先要判断这个返回值是否为 null，只有在非空的前提下才能将其作为其他方法的参数。这正是一些类似 Guava 的外部 API 试图解决的问题。
Optional 是 Java8 提供的为了解决 null 安全问题的一个 API。善用 Optional 可以使我们代码中很多繁琐、丑陋的设计变得十分优雅。
```java
public class Main {
    public static void main(String[] args) {

        User user = new User(88,"jesse");
        System.out.println(getName1(null));
        System.out.println(getName1(user));

        System.out.println(getName2(null));
        System.out.println(getName2(user));
    }
    // 传统方式
    public static String getName1(User u) {
        if (u == null){
            return "nknown";
        }
        return u.getName();
    }
    // 使用 Optional 方式
    public static String getName2(User user){
        // 为指定的值创建一个 Optional，如果指定的值为 null，
        // 则返回一个空的 Optional
        return Optional.ofNullable(user)
                // 这里的 u 就是上面的 user
                .map(u->u.getName())
                // 如果 user 为 null 直接到这一步
                .orElse("unknown");
    }
}
class User{
    private int id;
    private String name;
    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }
    public String getName() {
        return name;
    }
}
```

## 十二、`Queue` `Deque` `Stack`
- queue 队列，先进先出，不支持迭代器。队列通常（但并非一定）以 FIFO（先进先出）的方式排序各个元素。Queue 使用时要尽量避免 Collection 的 add() 和 remove() 方法，而是要使用 `offer()` 来加入元素，使用 `poll()` 来获取并移出元素。它们的优点是通过返回值可以判断成功与否，add() 和 remove() 方法在失败的时候会抛出异常。 如果只是使用而不移出该元素，使用 element() 或者 peek() 方法

- deque 双端队列，支持迭代器

- stack 堆栈，后进先出（LIFO）

```java
public class Main {
    public static void main(String[] args) {
        // Queue 单向队列
        Queue<String> queue = new LinkedList<String>();
        // 插入数据
        queue.offer("Hello");
        queue.offer("World!");
        queue.offer("你好！");
        queue.forEach(s->System.out.print(s+" "));
        System.out.println();
        // 检阅数据(数据还在)
        System.out.println("peek() -> " + queue.peek());
        // 移除数据
        System.out.println("poll() -> " + queue.poll());
        queue.forEach(s->System.out.print(s+" "));

        // Deque 双向队列
        Deque<Object> deque = new LinkedList<Object>();
        // 添加
        deque.offerFirst(1);
        deque.offerFirst(2);
        deque.offerLast("a");
        deque.offerLast("b");
        System.out.println();
        deque.forEach(s->System.out.println(s));
        // 检阅
        deque.peekFirst();
        deque.peekLast();
        deque.forEach(s->System.out.println(s));
        // 移除
        deque.pollFirst();
        deque.pollLast();
        deque.forEach(s->System.out.println(s));// 1，a

        // Stck
        Stack st = new Stack();
        // 压栈
        st.push("wce");
        st.push(12.8f);
        st.push(true);
        // 出栈
        st.pop();// true
        System.out.println(st);
    }
}
```

## 十三、Guava
Guava 工程包含了若干被 Google 的 Java 项目广泛依赖 的核心库，例如：集合 [collections] 、缓存 [caching] 、原生类型支持 [primitives support] 、并发库 [concurrency libraries] 、通用注解 [common annotations] 、字符串处理 [string processing] 、I/O 等等

## 十四、程序、进程、线程
- 程序: 指令集合，是进程静态描述文本，只有加载到内存中才能运行
- 进程: 程序在系统上顺序执行时的动态活动，有独立的地址空间和资源
- 线程: 是进程中不同的执行路径，可以并发执行，线程之间共享进程资源，cpu 最小执行单位，依赖于进程，不能独立运行。如果其中一个线程挂了，整个进程就蹦了。
- java 程序至少两线程，一个是 main 线程，另一个是 GC(垃圾回收)线程
- 创建线程方法
```java
public class Main {
    public static void main(String[] args) {

        // 继承 Theread 类
        MyThread1 myThread1 = new MyThread1();
        // 设置为守护线程，也就是当 进程中唯一的线程是守护线程时，jvm 将退出
        // 其它线程结束时，这时 myThread1 还在运行时，程序就会直接退出
        myThread1.setDaemon(true);
        myThread1.start();

        // 实现 Runnable 接口
        MyThread2 myThread2 = new MyThread2();
        Thread thread2 = new Thread(myThread2);
        thread2.start();

        for (int i = 0; i <10 ; i++) {
            System.out.println("主线程: " + i);
            if(i == 8){
                try {
                    // 在主线程运行 i== 3 时，让 myThread1 线程先执行完
                    myThread1.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }else if(i == 3){
                // 外部设置中断标志
                myThread1.interrupt();
            }else if(i ==6){
                // 让出本次 cpu 执行时间片
                Thread.yield();
            }
        }
    }
}
// 继承 Theread 类
class MyThread1 extends Thread{
    public void run(){
        // do something．．．
        for (int i = 0; i < 10; i++) {
           System.out.println("继承 Thread 线程: " + i);
           if(Thread.currentThread().isInterrupted()){
               System.out.println("XXXXX 继承 Thread 线程检测到终端标志");
               break;
           }
            try {
                // 休眠 500ms
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
                // 在异常中多加一句设置终端标志，是避免在休眠状态下，
                // 外部设置中断标志，会报异常，导致中断标志被清除
                Thread.currentThread().interrupt();
            }
        }
    }
}
// 实现 Runnable 接口
class MyThread2 implements Runnable{
    public void run(){
        // do something．．．
        for (int i=0;i<10;i++) {
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("实现 Runnable 接口: " + i);
        }
    }
}
```
- 线程同步方法
```java
public class Main {
    public static void main(String[] args) {

        MyThread2 myThread = new MyThread2();
        Thread thread22 = new Thread(myThread);
        Thread thread33 = new Thread(myThread);
        thread22.start();
        thread33.start();

    }
}

// 实现 Runnable 接口
class MyThread2 implements Runnable{
    private int money = 10;
    // 线程锁
    ReentrantLock lock = new ReentrantLock();

    public void run(){
        // 线程同步方法 1
        synchronized (this){
            for (int i=0;i<10;i++) {
                if(money > 0){
                    try {
                        // 模拟花钱时间
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(money--);
                      // 线程同步方法 3
//                    lock.lock();
//                    // ...
//                    lock.unlock();
                }
            }
        }
    }
    // 线程同步方法 2
    private synchronized void method(){
        // ...
    }
}
```
- 线程死锁: 在一个同步方法中调用另一个对象的同步方法，可能产生死锁
- 生产者和消费者
- 线程池
如果并发的线程数量很多，并且每个线程都是执行一个时间很短的任务就结束了，这样频繁创建线程就会大大降低系统的效率，因为频繁创建线程和销毁线程需要时间。在 Java 中可以通过线程池来达到这样的效果。


## 十五、网络编程
- 端口范围: `0 - 65535`，其中 `0-1023` 之间的端口是用于一些知名网络服务和应用

- 程序开发结构:
    - C/S(客户端/服务器): 开发两套程序，两套程序需要同时维护，例如: QQ。程序一般比较稳定。
    - B/S(浏览器/服务器): 开发一套，客户端使用浏览器。例如：论坛。BS 程序一般稳定性较差。

- UDP(User Datagram Protocol): 每个被传输的数据必须限定在 64KB 之内。

- URL(Uniform Resource Location): 统一资源定位符，表示互联网上的资源，如网页或者 FTP 地址。

- MINA 框架
Apache 的 Mina（Multipurpose Infrastructure Networked Applications）是一个网络应用框架，可以帮助用户开发高性能和高扩展性的网络应用程序；它提供了一个抽象的、事件驱动的异步 API，使 Java NIO 在各种传输协议（如TCP/IP，UDP/IP 协议等）下快速高效开发l
- [下载地址](http://mina.apache.org/downloads-mina.html)

## 十六、反射 与 内省
### 16.1 反射
- 反射(reflection): JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

- JavaBean: 就是 java 组件，广泛理解就是一个类。对于组件来说，关键要具有能够被 IDE 构建工具侦测其属性和事件的能力。可以被反射的能力。

- JavaBean 模型
![](http://images.jessechiu.com/javabean-model.png)



### 16.2 内省
- 内省(introspector): 是 java 语言对 Bean 类属性、事件的一种缺省处理方法。例如对属性 name 的 setName 和 getName 来获取和设置其值，这个就是默认规则。

### 16.3 AOP 框架
- AOP(Aspect Oriented Programming) 面向切面编程: 目的是可以重点关注核心代码，辅助部分可以通过动态代理方式实现。
- 应用场景: 权限验证，缓存，错误，调试...


## 十七、Annontation
- 定义: Annontation 是 Java5 开始引入的新特征。中文名称一般叫注解。它提供了一种安全的类似注释的机制，用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。更通俗的意思是为程序的元素（类、方法、成员变量）加上更直观更明了的说明，这些说明信息是与程序的业务逻辑无关，并且是供指定的工具或框架使用的。
Annontation 像一种修饰符一样，应用于包、类型、构造方法、方法、成员变量、参数及本地变量的声明语句中。

- 用途: Annotation 一般作为一种辅助途径，应用在软件框架或工具中，在这些工具类中根据不同的 Annontation 注解信息采取不同的处理过程或改变相应程序元素（类、方法及成员变量等）的行为。例如：Junit、Struts、Spring 等流行工具框架中均广泛使用了 annontion。使代码的灵活性大提高。


## 十八、XML 和 JSON
XML(Extensible Markup Language): 可扩展标记语言，用来描述数据文档

### 18.1 XML 文档解析
- SAX(Simple API for XML): 事件驱动方式，读取和操作 XML 数据更快的，更轻量的方法。主要用于移动端(Android)
- DOM 方式解析 XML
- JDOM 解析 XML
- DOM4J 解析 XML: 性能好，推荐使用

### 18.2 XML 文档生成
- XStream

### 18.3 JSON 文档解析
- JsonReader
- Gson 直接把 JSON 数据转换为 java 对象
