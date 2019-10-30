---
title: Java 代码实例
date: 2018-04-10 09:49:25
categories: Java
---
本文主要是收集 java 简单代码实例，可以拿来当作初学练习

## 一、猜数字
```java
package com.company;
import java.util.Random;
import java.util.Scanner;
public class Main {
    public static void main(String[] args) {
        Random random = new Random();
        // 存储随机生成的数字
        int[] num = new int[5];
        // TODO 生成不重复的随机数
        for (int i = 0; i < num.length; i++) {
            // 生成一个 [0,20) 以内的随机数
            num[i] = random.nextInt(20);
//            System.out.println(num[i]);
        }
        // 创建一个屏幕输入实例
        Scanner input = new Scanner(System.in);
        System.out.println("请输入你要猜的数字：(20 以内)");
        int times = 0;// 计算猜数的次数

        while(true){
            // 等待屏幕输入
            int inputNum = input.nextInt();
            boolean flag = false;// 是否猜对标志
            for (int i:num){
                if (i == inputNum) {
                    flag = true;
                    break;
                }else{
                    flag = false;
                }
            }
            if (flag) {
                System.out.println("在经过 "+ times + " 次的努力，恭喜你猜对了!");
                break;
            }else{
                times++;
                System.out.println("猜错了，请再来一次！");
            }
        }
    }
}
```

## 二、查找指定目录下指定文件
```java
public class Main {
    private static int num = 0;
    public static void main(String[] args) {
        // File 类代表一个文件 或 目录
//        findFile(new File("D:\\hexo\\hexo-blog-gitlab"),".js");
        findFile(new File("D:"+File.separator+"hexo"+File.separator+"hexo-blog-gitlab"),".js");
    }

    /**
     * 查找指定目录下指定文件
     * @param target
     * @param ext
     */
    private static void findFile(File target, String ext){
        if (target == null) {
            return ;
        }
        // 如果是一个目录
        if (target.isDirectory()) {
            // 返回该目录下所有文件对象(返回的文件对象列表)
            File[] files = target.listFiles();
            for (File f: files) {
                // 递归调用
                findFile(f,ext);
            }
        }else{
            // 这里表示 File 是一个文件
            // 获取文件名，并统一转为小写
            String name = target.getName().toLowerCase();
            // 过虑指定后最名文件
            if(name.endsWith(ext.toLowerCase())){
                // 打印文件的绝对路径
                System.out.println((num++) + " " + target.getAbsolutePath());
            }
        }
    }
}
```

## 三、文件拷贝

### 1. 使用字节流方式

```java
public class Main {
    private static int num = 0;
    public static void main(String[] args) throws IOException {
        copyFile("D:\\girl.jpg","D:\\girl-copy.jpg");
    }
    /**
     * 文件拷贝，使用字节流方式
     * @param src
     * @param dist
     */
    private static void copyFile(String src, String dist) throws IOException {
        File srcFile = new File(src);
        File distFile = new File(dist);
        // 输入输出每次只会操作一个字节(从文件读取和写入)
        InputStream in = null;
        OutputStream out = null;
        try {
            // 创建输入流实例
            in = new FileInputStream(srcFile);
            // 创建输出流实例
            out = new FileOutputStream(distFile);
            // 创建字节数组缓存
            byte[] bytes = new byte[1024];
            int len = -1;
            // 循环读取写入
            while((len = in.read(bytes)) != -1){
                out.write(bytes,0,len);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (in != null) {
                in.close();
            }
            if (out != null) {
                out.close();
            }
        }
    }
}
```

### 2. 使用字符流方式

```java
public class Main {
    private static int num = 0;
    public static void main(String[] args) throws IOException {
        copyFile("D:\\test.js","D:\\test-copy.js");
    }
    /**
     * 文件拷贝，使用字符流方式
     * @param src
     * @param dist
     */
    private static void copyFile(String src, String dist) throws IOException {
        File srcFile = new File(src);
        File distFile = new File(dist);
        // 每次操作单位是一个字符，内部自带缓存，默认大小 1024 字节，
        // 缓存满后，手动刷新，关闭时会把数据写入文件
        Reader in = null;
        Writer out = null;
        try {
            // 创建输入流实例
            in = new FileReader(srcFile);
            // 创建输出流实例
            out = new FileWriter(distFile);
            // 创建读取单个字符缓存
            char[] cs = new char[1];
            // 创建字符总缓存 buf
            StringBuilder buf = new StringBuilder();
            int len = -1;
            // 循环读取写入
            while((len = in.read(cs)) != -1){
                buf.append(new String(cs,0,len));
            }
            // 写入文件
            out.write(buf.toString());
//            System.out.println("读取的内容: " + buf);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (in != null) {
                in.close();
            }
            if (out != null) {
                out.close();
            }
            System.out.println("文件拷贝完成");
        }
    }
}

```

### 3. 使用 NIO(New IO) 方式
```java
public class Main {
    public static void main(String[] args) {
        RandomAccessFile fromFile = null;
        try {
            fromFile = new RandomAccessFile("d:\\test.txt", "rw");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        FileChannel fromChannel = fromFile.getChannel();
        RandomAccessFile toFile = null;
        try {
            toFile = new RandomAccessFile("d:\\toFile.txt", "rw");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        FileChannel toChannel = toFile.getChannel();
        long position = 0;
        long count = 0;
        try {
            count = fromChannel.size();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            fromChannel.transferTo(position, count, toChannel);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 四、文件分割和合并
```java
class Main {
    public static void main(String[] args) throws IOException {
        // 分割文件
        splitFile("D:\\test.mp4");
        // 合并文件
        unionFile("D:\\test-unio.mp4");
    }

    /**
     * 合并文件
     * @param dstName
     * @throws IOException
     */
    public static void unionFile(String dstName) throws IOException {
        // 创建输出字节流实例
        FileOutputStream out = new FileOutputStream(dstName);

        // Vector 效率低,改为 ArrayList
        ArrayList<FileInputStream> al = new ArrayList<FileInputStream>();

        //利用 File 遍历文件夹下的文件
        File dir = new File("D:\\split");

        // 获取文件夹下所有文件实例
        File[] files = dir.listFiles();
        for (int x = 0; x < files.length; x++) {
            // 以输入流的方式，按顺序添加到 al 数组中
            al.add(new FileInputStream(files[x]));
        }

        // ArrayList 本身没有枚举方法，通过迭代器来实现
        final Iterator<FileInputStream> it = al.iterator();

        // 匿名内部类，复写枚举接口下的两个方法
        Enumeration<FileInputStream> en = new Enumeration<FileInputStream>() {
            public boolean hasMoreElements() {
                return it.hasNext();
            }
            public FileInputStream nextElement() {
                return it.next();
            }
        };
        // 排序文件流
        SequenceInputStream sis = new SequenceInputStream(en);
        // 实际读取长度变量
        int readLen = 0;
        // 定义1M的缓存区
        byte[] buf = new byte[1024 * 1024];
        while ((readLen = sis.read(buf)) != -1) {
            out.write(buf, 0, readLen);
        }
        sis.close();
        out.close();
        System.out.println("合并完成");
    }

    /**
     * 分割文件
     * @param fileName：待分割的文件名
     * @throws IOException
     */
    public static void splitFile(String fileName) throws IOException {
        // 创建输入字节流实例
        FileInputStream  in = new FileInputStream(fileName);
        // 创建输出字节流实例
        FileOutputStream out = null;
        // 分割文件大小 1M
        byte[] buf = new byte[1024 * 1024];
        // 定义实际读取长度变量
        int readLen=-1;
        // 分割文件的序号
        int index = 0;
        while ((readLen = in.read(buf)) != -1) {
            out = new FileOutputStream("D:\\split\\" + (index++) + ".part");
            out.write(buf, 0, readLen);
            out.flush();
            out.close();
        }
        in.close();
        System.out.println("分割结束");
    }
}
```

## 五、properties 文件读写
```java
public class Main {
    // 根据 Key 读取 Value
    public static String getValueByKey(String filePath, String key) {
        // 实例化 properties 
        Properties pps = new Properties();
        try {
            // 创建带缓存的输入流实例
            InputStream in = new BufferedInputStream (new FileInputStream(filePath));
            // 加载文件
            pps.load(in);
            // 通过属性值(key) 读取值(value)
            String value = pps.getProperty(key);
            System.out.println("getValueByKey: " + key + " = " + value);
            return value;
        }catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }

    // 读取 Properties 的全部信息
    public static void getAllProperties(String filePath) throws IOException {
        Properties pps = new Properties();
        InputStream in = new BufferedInputStream(new FileInputStream(filePath));
        pps.load(in);

        // 获取配置文件中所有 key 的值
        Enumeration en = pps.propertyNames(); 
        // 遍历 key 值，并获取对应的 value
        while(en.hasMoreElements()) {
            String strKey = (String) en.nextElement();
            String strValue = pps.getProperty(strKey);
            System.out.println("getAllProperties: " + strKey + " = " + strValue);
        }
    }

    // 写入 Properties 信息
    public static void writeProperties (String filePath, String pKey, String pValue) throws IOException {
        Properties pps = new Properties();
        InputStream in = new FileInputStream(filePath);
        pps.load(in);
        // 设置 key=value
        pps.setProperty(pKey, pValue);

        // 创建输出字节流
        OutputStream out = new FileOutputStream(filePath);
        // 重新保存文件
        pps.store(out, "Update " + pKey + " name");
    }

    public static void main(String [] args) throws IOException{
        String fileName = "d:\\test.properties";
        // 根据 key 获取值
        getValueByKey(fileName,"name");
        // 获取所有值
        getAllProperties(fileName);
        // 写入 key=value
        writeProperties(fileName,"name", "lili");
    }
}
```

## 六、简易聊天小程序


### 1. TCP/IP
- 客户端 Client.java
```java
- 一个简单的聊天程
public class Client {
    public static void main(String[] args) {
        try {

            // 1.创建客户端 Socket，指定服务器地址和端口
            Socket socket=new Socket("localhost", 8888);

            // 2.获取输出流，向服务器端发送信息
            OutputStream os = socket.getOutputStream();// 字节输出流
            PrintWriter pw = new PrintWriter(os);// 将输出流包装为打印流
            pw.write("Hello Server，I am coming");
            pw.flush();
            socket.shutdownOutput();//关闭输出流

            // 3.获取输入流，并读取服务器端的响应信息
            InputStream is = socket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String info = null;
            while((info = br.readLine()) != null){
                System.out.println("服务器说：" + info);
            }

            // 4.关闭资源
            br.close();
            is.close();
            pw.close();
            os.close();
            socket.close();

        } catch (UnknownHostException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- 服务端代码 Server.java
```java
class ServerThread extends Thread {
    // 和本线程相关的 Socket
    Socket socket = null;

    public ServerThread(Socket socket) {
        this.socket = socket;
    }

    // 线程执行的操作，响应客户端的请求
    public void run() {
        InputStream is = null;
        InputStreamReader isr = null;
        BufferedReader br = null;
        OutputStream os = null;
        PrintWriter pw = null;
        try {
            //获取输入流，并读取客户端信息
            is = socket.getInputStream();
            isr = new InputStreamReader(is);
            br = new BufferedReader(isr);
            String info = null;
            while ((info = br.readLine()) != null) {// 循环读取客户端的信息
                System.out.println("客户端说：" + info);
            }
            socket.shutdownInput();//关闭输入流

            // 获取输出流，响应客户端的请求
            os = socket.getOutputStream();
            pw = new PrintWriter(os);
            pw.write("欢迎您！");
            // 调用 flush() 方法将缓冲输出
            pw.flush();

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 关闭资源
            try {
                if (pw != null)
                    pw.close();
                if (os != null)
                    os.close();
                if (br != null)
                    br.close();
                if (isr != null)
                    isr.close();
                if (is != null)
                    is.close();
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- 测试代码 Main.java
```java
public class Main {
    public static void main(String[] args) {
        try {
            // 1.创建一个服务器端 Socket，即 ServerSocket，指定绑定的端口，并监听此端口
            ServerSocket serverSocket = new ServerSocket(8888);
            Socket socket = null;
            // 记录客户端的数量
            int count = 0;
            System.out.println("***服务器即将启动，等待客户端的连接***");

            // 循环监听等待客户端的连接
            while (true) {
                // 调用 accept() 方法开始监听，等待客户端的连接
                socket = serverSocket.accept();
                // 创建一个新的线程
                ServerThread serverThread = new ServerThread(socket);
                // 启动线程
                serverThread.start();
                count++;// 统计客户端的数量
                System.out.println("当前连接客户端的数量：" + count);
                InetAddress address = socket.getInetAddress();
                System.out.println("当前客户端的IP：" + address.getHostAddress());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
### 2. UDP
- Client
```java
public class Client {
    public static void main(String[] args) throws IOException {
        //*** 发送数据***/
        // 初始化datagramSocket,注意与前面Server端实现的差别
        DatagramSocket datagramSocket = new DatagramSocket();

        // 创建一个屏幕输入实例
        Scanner input = new Scanner(System.in);
        while (true) {
            System.out.println("请输入要发送数据");
            // 等待屏幕输入
            String inputInfo = input.nextLine();

            // 使用 DatagramPacket(byte buf[], int length, InetAddress address, int port) 函数组装发送UDP数据报
            InetAddress address = InetAddress.getByName("localhost");
            DatagramPacket datagramPacket = new DatagramPacket(inputInfo.getBytes(), inputInfo.length(), address, 3637);

            // 发送数据
            datagramSocket.send(datagramPacket);

            /*** 接收数据***/
            byte[] receBuf = new byte[1024];
            DatagramPacket recePacket = new DatagramPacket(receBuf, receBuf.length);
            datagramSocket.receive(recePacket);

            String receStr = new String(recePacket.getData(), 0, recePacket.getLength());
            System.out.println("Client Rece Ack:" + receStr);
            System.out.println(recePacket.getPort());
        }
    }
}

```
- Server
```java
public static void udpServer() throws IOException {
    // 创建一个数据套接字，并绑定到 3637 接口
    DatagramSocket datagramSocket = new DatagramSocket(3637);
    // 用于接收数据
    byte[] receive = new byte[1024];
    DatagramPacket datagramPacket = new DatagramPacket(receive, receive.length);

    while (true) {
        System.out.println("udp 服务等带接收数据...");

        // 等待接收数据
        datagramSocket.receive(datagramPacket);

        /****** 解析数据报****/
        String receStr = new String(datagramPacket.getData(), 0, datagramPacket.getLength());
        System.out.println("Server Rece:" + receStr);
        System.out.println("Server Port:" + datagramPacket.getPort());

        /***** 返回ACK消息数据报*/
        // 组装数据报
        byte[] buf = "I receive the message".getBytes();
        DatagramPacket sendPacket = new DatagramPacket(buf, buf.length, datagramPacket.getAddress(), datagramPacket.getPort());
        // 发送消息
        datagramSocket.send(sendPacket);
    }
}
```

- 测试代码
```java
public class Main {
    public static void main(String[] args) throws IOException {
        udpServer();
    }
}
```

## 七、URL 下载图片
```java
public class Main {
    public static void main(String[] args) throws IOException {

        URL url = new URL("https://images2018.cnblogs.com/news/24442/201804/24442-20180402101833163-1165053275.jpg");
        HttpURLConnection urlcon = (HttpURLConnection) url.openConnection();
        urlcon.connect();//获取连接

        System.out.println(" content-encode：" + urlcon.getContentEncoding());
        System.out.println(" content-length：" + urlcon.getContentLength());
        System.out.println(" content-type：" + urlcon.getContentType());
        System.out.println(" date：" + urlcon.getDate());

        // 获取输入流
        InputStream is = urlcon.getInputStream();
        File out = new File("d:\\temp.jpg");
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(out));
        byte[] buffer = new byte[1024];
        int len = -1;
        while ((len = is.read(buffer)) != -1) {
            bos.write(buffer, 0, len);
            bos.flush();
        }
        System.out.println("下载结束...");
    }
}
```


## 参考
- [Java NIO(New IO) 通俗易懂简明教程](https://blog.csdn.net/u011263794/article/details/51789530)