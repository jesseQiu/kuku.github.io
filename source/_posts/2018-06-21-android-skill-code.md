---
title: Android 代码实例
date: 2018-06-21 14:34:39
updated: 2018-06-21 14:34:39
categories: Android
---

## 一、MD5
```java
private String getMd5String(String input){
      MessageDigest md = null;
      try {
          md = MessageDigest.getInstance("md5");
      } catch (NoSuchAlgorithmException e) {
          e.printStackTrace();
      }

      String result="";
      try {
          byte[] mdBytes = md.digest(input.getBytes("UTF-8"));
          for (byte b : mdBytes) {
              String temp = Integer.toHexString(b & 0xff);
              if (temp.length() == 1) {
                  temp = "0" + temp;
              }
              result += temp;
          }
      } catch (UnsupportedEncodingException e) {
          e.printStackTrace();
      }

      return result;
  }
```

## 二、显示弹框
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

