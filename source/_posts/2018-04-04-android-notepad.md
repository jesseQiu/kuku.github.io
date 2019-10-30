---
title: Android Notepad
date: 2018-04-04 09:49:25
categories: Android
---
>本文是在学习的  Android 时跟着慕课网教程[Android 记账本](https://www.imooc.com/learn/803),码的代码，其中主要涉及了以下几块知识点，这里做下梳理和备忘。

- 适配层的应用
- 数据库 SQL 的应用
- 浮动按键应用
- 弹框应用
- 菜单应用
- 两个 Active 之间调用，以及传值
- 第三方库的引用方法
- 快捷键使用
- 样式布局应用

![](http://images.jessechiu.com/notepad%281%29.png)
![](http://images.jessechiu.com/notepad%282%29.png)
![](http://images.jessechiu.com/notepad%283%29.png)
![](http://images.jessechiu.com/notepad%284%29.png)

## 一、适配层
```java
// CostListAdapter.java
public class CostListAdapter extends BaseAdapter {
    private List<CostBean> mList;
    private Context mcontext;
    private LayoutInflater mLayoutInflater;

    // 构造函数
    public CostListAdapter(Context context, List<CostBean> mList){
        this.mcontext = context;
        this.mList = mList;
        // 存储布局数据
        this.mLayoutInflater = LayoutInflater.from(context);
    }

    @Override
    public int getCount() {
        return this.mList.size();
    }

    @Override
    public Object getItem(int i) {
        return this.mList.get(i);
    }

    @Override
    public long getItemId(int i) {
        return i;
    }

    @Override
    public View getView(int i, View view, ViewGroup parent) {
        ViewHolder holder = null;
        if(view == null){
            // 把 list_item 样式应用上
            view = mLayoutInflater.inflate(R.layout.list_item,null);
            holder = new ViewHolder();
            holder.mTvConstTitle = (TextView) view.findViewById(R.id.tv_title);
            holder.mTvConstDate = (TextView) view.findViewById(R.id.tv_date);
            holder.mTvConstMoney = (TextView) view.findViewById(R.id.tv_costMoney);
            view.setTag(holder);
        }else{
            // 如果已经有缓存了，直接取出来
            holder = (ViewHolder) view.getTag();
        }
        CostBean bean = mList.get(i);
        holder.mTvConstTitle.setText(bean.costTitle);
        holder.mTvConstDate.setText(bean.costDate);
        holder.mTvConstMoney.setText(bean.costMoney);
        return view;
    }
    
    private static class ViewHolder {
        TextView mTvConstTitle;
        TextView mTvConstDate;
        TextView mTvConstMoney;
    }
}


// 数据显示
ListView costList = findViewById(R.id.lv_main);
costListAdapter = new CostListAdapter(this, mCostBeanList);
costList.setAdapter(costListAdapter);

// 通知 adapter 更新数据
costListAdapter.notifyDataSetChanged();
```


## 二、数据库 SQL 的应用
```java
// 继承 SQL 接口类
public class DataBaseHelper extends SQLiteOpenHelper {
    public static final String NOTEPAD_DB = "notepad";
    public static final String COST_TITLE = "cost_title";
    public static final String COST_DATE = "cost_date";
    public static final String COST_MONEY = "cost_money";

    // 构造函数
    public DataBaseHelper(Context context){
        // 创建数据库
        super(context, NOTEPAD_DB,null,1);
    }
    @Override
    public void onCreate(SQLiteDatabase db) {
        // 创建表格
        // 注意字符串拼接
        db.execSQL("create table if not exists notepad(" +
                "id integer primary key," +
                COST_TITLE + " varchar," +
                COST_DATE + " varchar," +
                COST_MONEY + " varchar)");
    }

    // 插入数据方法
    public void insert(CostBean bean){
        SQLiteDatabase db = getWritableDatabase();
        ContentValues cv = new ContentValues();

        cv.put(COST_TITLE,bean.costTitle);
        cv.put(COST_DATE,bean.costDate);
        cv.put(COST_MONEY,bean.costMoney);

        db.insert(NOTEPAD_DB,null,cv);
    }

    // 数据查询
    public Cursor getAllCursorDate(){
        SQLiteDatabase db = getWritableDatabase();
        return db.query(NOTEPAD_DB,null,null,null,null,null, COST_DATE + " ASC");
    }

    // 删除所有数据
    public void deleteAllData(){
        SQLiteDatabase db = getWritableDatabase();
        db.delete(NOTEPAD_DB,null,null);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

    }
}

// 定义变量
private DataBaseHelper mDataBaseHelper;
mDataBaseHelper = new DataBaseHelper(this);

// 获取数据库所有数据
Cursor cursor = mDataBaseHelper.getAllCursorDate();
if(cursor != null){
    while(cursor.moveToNext()){
        CostBean costBean = new CostBean();
        costBean.costTitle = cursor.getString(cursor.getColumnIndex("cost_title"));
        costBean.costDate = cursor.getString(cursor.getColumnIndex("cost_date"));
        costBean.costMoney = cursor.getString(cursor.getColumnIndex("cost_money"));
        mCostBeanList.add(costBean);
    }
    cursor.close();
}

// 插入数据库
mDataBaseHelper.insert(costBean);
```

## 三、浮动按键应用
```java
private void initFloatBtn(){
    // 设置浮动按键事件
    FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
    fab.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
        		// TODO
        }
    });
}
```

## 四、弹框应用
```java
// 注意这里实例化 builder this 作用域
AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
LayoutInflater inflater = LayoutInflater.from(MainActivity.this);

// 设置 view 样式布局
View viewDialog = inflater.inflate(R.layout.new_cost_data,null);
// 获取布局元素
final EditText title = (EditText) viewDialog.findViewById(R.id.et_cost_title);
final EditText money = (EditText) viewDialog.findViewById(R.id.et_cost_money);
final DatePicker date = (DatePicker) viewDialog.findViewById(R.id.dp_cost_date);

// 设置标题
builder.setTitle("New Cost");
// 显示弹框
builder.setView(viewDialog);

// 绑定点击 OK 按键事件
builder.setPositiveButton("Ok", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int i) {
        CostBean costBean = new CostBean();
        costBean.costTitle = title.getText().toString();
        costBean.costMoney = money.getText().toString();
        // 注意这里的日期拼接
        costBean.costDate = date.getYear()+"-"+(date.getMonth()+1)+"-"+date.getDayOfMonth();
        // 插入数据库
        mDataBaseHelper.insert(costBean);
        // 显示列表中添加
        mCostBeanList.add(costBean);
        // 通知 adapter 更新数据
        costListAdapter.notifyDataSetChanged();
    }
});

// 取消事件
builder.setNegativeButton("Cancel",null);
// 显示对话框
builder.create().show();
```

## 五、菜单应用
```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    // Inflate the menu; this adds items to the action bar if it is present.
    // 设置工具条右边菜单项
    getMenuInflater().inflate(R.menu.menu_main, menu);
    return true;
}

// 顶部菜单事件
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();
    if (id == R.id.action_chart) {
        return true;
    }
    return super.onOptionsItemSelected(item);
}
```

## 六、两个 Active 之间调用，以及传值
```java
// 调用另一个 active
Intent intent = new Intent(this,ChartsActivity.class);
// 序列化数据，方可通过 intent 传递
intent.putExtra("cost_list",(Serializable) mCostBeanList);
startActivity(intent);

// 在子 Active 中，添加返回图标
// AndroidManifest.xml
<activity
    android:name=".ChartsActivity"
    android:label="chart">
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity"
        />
</activity>
```

## 七、第三方库的引用方法
```java
// build.gradle 文件中
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:27.1.0'
    implementation 'com.android.support.constraint:constraint-layout:1.0.2'
    implementation 'com.android.support:design:27.1.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.1'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'
    // 图表库
    implementation 'com.github.lecho:hellocharts-library:1.5.8@aar'
}
```

## 八、完整代码
**[github 地址](https://github.com/Jesse-Chiu/Android-Notepad)**

## 参考
- [Android 记账本](https://www.imooc.com/learn/803)
