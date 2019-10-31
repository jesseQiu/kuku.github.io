---
title: Android 代码实例
date: 2018-06-21 14:34:39
updated: 2018-06-21 14:34:39
categories: Android
---

## 显示弹框
### 1. 普通弹框
```java
private void showInputTimeDialog(){
    AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
    builder.setIcon(R.mipmap.ic_launcher);
    builder.setTitle("请输入采集日志持续时间");

    //  通过 LayoutInflater 来加载一个 xml 的布局文件作为一个 View 对象
    View view = LayoutInflater.from(MainActivity.this).inflate(R.layout.dialog_time_value, null);
    // 设置我们自己定义的布局文件作为弹出框的 Content
    builder.setView(view);
    final EditText durationTime = (EditText)view.findViewById(R.id.duriationTime);
    builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialog, int which) {
            String time = durationTime.getText().toString().trim();
            if(time.equals("")){
                Toast.makeText(MainActivity.this,"Please enter the value",Toast.LENGTH_SHORT).show();
                return;
            }
            initAcquisitionLog(Integer.parseInt(time));
        }
    });
    builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialog, int which) {
        }
    });
    builder.show();
}
```
### 2. 设置系统级别弹框
有的时候我们需要非 Activity 的类下面弹框，例如在广播接收器(receiver)中弹框，这时只能是系统级别的弹框才行。
```java
private void showInputTimeDialog(final Context context,final String filePath){
    AlertDialog.Builder builder = new AlertDialog.Builder(context);
    builder.setIcon(R.mipmap.ic_launcher);
    builder.setTitle("请输入发送的邮箱地址");

    //  通过 LayoutInflater 来加载一个 xml 的布局文件作为一个 View 对象
    View view = LayoutInflater.from(context).inflate(R.layout.dialog_send,null);

    // 设置我们自己定义的布局文件作为弹出框的 Content
    builder.setView(view);
    final EditText emailText = (EditText)view.findViewById(R.id.email);
    builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
        @Override
        public void onClick(DialogInterface dialog, int which) {
            String emailAddress = emailText.getText().toString().trim();
            if (!MailHelper.verifyEmailAddress(emailAddress)) {
                Toast.makeText(context, "请输入正确的邮箱地址", Toast.LENGTH_SHORT).show();
                return;
            }
            sendLogEmail(filePath, emailAddress);
        }
    });

    AlertDialog alertDialog = builder.create();
    // 需要设置为系统基本弹框
    alertDialog.getWindow().setType(WindowManager.LayoutParams.TYPE_SYSTEM_ALERT);
    alertDialog.show();
}
```
在 `AndroidManifest.xml` 文件中添加 `<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />`


## 权限检查三种方法
```java
    public static boolean checkPermission1(Context context, String[] permissions) {
        PackageManager packageManager = context.getPackageManager();
        String packageName = context.getPackageName();

        for (String permission : permissions) {
            int per = packageManager.checkPermission(permission, packageName);
            if (PackageManager.PERMISSION_DENIED == per) {
                Log.w(TAG, "required permission not granted . permission = " + permission);
                return false;
            }
        }
        return true;
    }

    public static boolean checkPermission2(Context context, String[] permissions) {

        for (String permission : permissions) {
            int per =context.checkPermission(permission, Process.myPid(),Process.myUid());
            if (PackageManager.PERMISSION_GRANTED != per) {
                Log.w(TAG, "required permission not granted . permission = " + permission);
                return false;
            }
        }
        return true;
    }

    public static boolean checkPermission3(Context context, String[] permissions) {

        for (String permission : permissions) {
            int per = ContextCompat.checkSelfPermission(context, Manifest.permission.CAMERA);
            if (PackageManager.PERMISSION_GRANTED != per) {
                Log.w(TAG, "required permission not granted . permission = " + permission);
                return false;
            }
        }
        return true;
    }
```
- [Android 检测权限的三种写法]https://www.jianshu.com/p/11d99a9be7d9)





## 主线程和子线程之间传递消息
- [android 主线程和子线程之间的消息传递](https://www.cnblogs.com/laughingQing/p/5436998.html)




## Activity

### Activity 之间传值
- [android 之Activity的五种传值方式](https://blog.csdn.net/qq_37169103/article/details/80406441)

### Activity 四种启动模式
- [细谈 Activity 四种启动模式](https://blog.csdn.net/zy_jibai/article/details/80587083)
- [终于懂了之 Activity 的四种启动模式](https://www.jianshu.com/p/50264d6cccb3?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)





## Service
服务和广播都是运行在 UI 线程中，如果需要操作耗时操作，需要在里面创建一个新的线程来运行

### 服务类型
1. 不可交互的后台服务：onCreate() -> onStartCommand() -> onDestroy()
2. 可交互的后台服务： onCreate() -> onBind() -> onUnbind() -> onDestory()
3. 前台服务
4. IntentService

### 跨应用 Service 通讯
#### 通过 Intent
通过 Action 的隐式 Intent 启动 Service 在安卓 5.0 以前的版本是可以实现跨应用启动 Service 的，安卓 5.0 以后的版本若想实现跨应用启动 Service 的功能必须使用显示 Intent
```java
serviceIntent = new Intent();
serviceIntent.setComponent(new ComponentName("com.example.service1", "com.example.service1.MyService"));
//...
startService(serviceIntent);
//...
stopService(serviceIntent);
```
- [Android -- 跨应用启动Service](https://blog.csdn.net/gaopeng0071/article/details/46048159)

#### AIDL 
- [Android 通过 AIDL 在两个 APP之 间 Service 通信]方式(https://www.cnblogs.com/xqz0618/p/aidl_service.html)


### 参考
- [Service 精炼详解](https://blog.csdn.net/weixin_41101173/article/details/79684183)
- [Android IntentService 使用介绍以及原理分析](https://www.jianshu.com/p/8c4181049564)
- [Android Service 与 Activity 之间通信的几种方式](https://blog.csdn.net/xiaanming/article/details/9750689)