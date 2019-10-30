---
title: Java 实用技能
date: 2018-04-11 09:49:25
categories: Java
---
## 一、数组 
- ### 1.1动态扩充数组
```java
public class Main {
    public static void main(String[] args) {
        int[] temp = {1,2,3,4,5};
        System.out.println("原来长度: " + temp.length);

        // 动态数组扩充
        temp = Arrays.copyOf(temp,temp.length*2);
        System.out.println("扩充一倍长度后: " + temp.length);
        for (int i = 0; i <temp.length ; i++) {
            System.out.println(temp[i]);
        }
    }
}
```
- ### 1.2 常用方法
- 定义数组：int[] score = {1,2,3,4,5}  等价于 int[] score = new int[]{1,2,3,4,5}
- 数组操作：Array 类(java.util 库中) Array.sort()、Array.toString();
- foreach 循环数组
```java
for(int i : score){
    System.out.println(i);
}
```

## 二、`String`
- String: 编译时值固定，不能修改
- StringBufer: 线程安全，适合在多线程中使用
- StringBuilder: 不是线程安全，但是性能高

## 三、`Cloneable`
对象需要具备克隆功能
1. 实现 Cloneable 接口
2. 重写 Object 类中的 clone 方法
```java
public class Main {
    public static void main(String[] args) {
        Cat cat = new Cat("白猫",2);
        try{
            // clone 操作需要在 try catch 内
            Cat newCat = (Cat) cat.clone();
            System.out.println("cat: " + cat);
            System.out.println("newCat: " + newCat);
        }catch(CloneNotSupportedException e){
            e.printStackTrace();
        }
    }
}
// 对象需要具备克隆功能
// 1. 实现 Cloneable 接口
// 2. 重写 Object 类中的 clone 方法;
class Cat implements Cloneable{
    private String name;
    private int age;
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public Cat(){};
    public Cat(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    @Override
    public String toString() {
        return "Cat{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

## 四、`System` 和 `RunTime` 类
### 4.1 System 
代表系统，系统级的很多的属性和控制方法都放置在该类内部，位于 java.lang 包
```java
public class Main {
    public static void main(String[] args) {

        // arraycopy: 数组拷贝
        int[] num1 = {1,2,3,4,5};
        int[] num2 = new int[num1.length];

        // 参数顺序: 源数组，源数组起始位置，目标数组，目标数组起始位置，长度
        System.arraycopy(num1,0,num2,0,num1.length);
        System.out.println(Arrays.toString(num2));

        // currentTimeMillis: 1970.1.1 到当前毫秒数
        System.out.println(System.currentTimeMillis());
        Date nowDate = new Date(System.currentTimeMillis());
        DateFormat df = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss");
        String now = df.format(nowDate);
        System.out.println(now);

        // exit: 退出虚拟机
        System.exit(0);

        // 获取系统信息
        System.out.println(System.getProperties());
    }
}
```

### 4.2 `RunTime`
每个 Java 应用程序都有一个 Runtime 类实例，使应用程序能够与其运行的环境相连接
```java
public class Main {
    public static void main(String[] args) {

        Runtime rt = Runtime.getRuntime();
        System.out.println("处理器数: " + rt.availableProcessors() + " 个");
        System.out.println("Jvm 总内存: " + rt.totalMemory()+" byte");
        System.out.println("Jvm 空闲空间: " + rt.freeMemory()+" byte");
        System.out.println("Jvm 可用最大内存数: " + rt.maxMemory()+" byte");

        // 在单独的进程中执行指定的字符串命令
        try{
            rt.exec("notepad");
        }catch(IOException e){
            e.printStackTrace();
        }

        // 加载 C\C++ 类库
//        System.loadLibrary("xxxx");
    }
}
```

## 五、MD5 工具类
MD5 全称是 Message Digest Alogrithm 5(信息摘要算法)
```java
public class Main {
    public static void main(String[] args) {
        String password = "lucky";
        System.out.println("原值: " + password);
        try {
            // 创建 md5 对象实例
            MessageDigest md = MessageDigest.getInstance("md5");
            // 计算 md5
            byte[] mdBytes = md.digest(password.getBytes("UTF-8"));
            System.out.println("md5： " + Arrays.toString(mdBytes));
            
            // 1.8 版本
//            String base64Str = Base64.getEncoder().encodeToString(mdBytes);

            // 1.8 之前
            BASE64Encoder base64Encoder = new BASE64Encoder();
            String base64Str = base64Encoder.encode(mdBytes);
            System.out.println("base64： " + base64Str);
            // base64 解码
            BASE64Decoder base64Decoder = new BASE64Decoder();
            byte[] deBase64 = base64Decoder.decodeBuffer(base64Str);
            System.out.println("base64 解码： " + Arrays.toString(deBase64));
            
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 六、线程安全的单列模式
```java
class Singleton implements Serializable{
    // 增加了 volatile 这样编译器会在每次读取该变量值时，都会从地址中读取，
    // 避免了在多线程中由于编译器优化暂时存放在寄存器中，导致读取值不一致问题。
    private volatile static Singleton singleton = null;
    private Singleton(){
        // 这里主要防止通过反射方式实例化对象
        if(singleton != null){
            throw new RuntimeException("此类对象为单列模式，已经被实例化!")
        }
    }
    public static Singleton getSingleton(){
        // 这层是判断是否已经 new 过
        if(singleton == null){
            // 使用当前类信息作为锁
            synchronized (Singleton.class){
                if(singleton == null){
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```


## x、常用类
- Date: java.util // 建议使用 Calendar 替代
- SimpleDateFormate(): java.text 


