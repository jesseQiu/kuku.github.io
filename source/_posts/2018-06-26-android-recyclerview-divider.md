---
title: RecyclerView 设置分割线方法
date: 2018-06-26 14:34:39
updated: 2018-06-26 14:34:39
categories: Android
---

## 一、最简单的方法（布局划线）
在 item.xml 文件中在最下方指定一条分割线
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical" android:layout_width="match_parent"  
    android:layout_height="wrap_content">  
    <LinearLayout  
        android:orientation="horizontal"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content">  
        <ImageView  
            android:id="@+id/image_view"  
            android:layout_gravity="center"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content" />  
        <TextView  
            android:id="@+id/text_view"  
            android:layout_gravity="center"  
            android:gravity="left"  
            android:layout_marginTop="10dp"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content" />  
    </LinearLayout>  

    <View  
        android:layout_width="match_parent"  
        android:layout_height="1dp"  
        android:background="@color/colorPrimary"/>
</LinearLayout>  
```

## 二、重载 DividerItemDecoration 类
使用方法
```java
1. 添加默认分割线：高度为2px，颜色为灰色
mRecyclerView.addItemDecoration(new RecycleViewDivider(mContext, LinearLayoutManager.VERTICAL));

2 添加自定义分割线：可自定义分割线drawable
mRecyclerView.addItemDecoration(new RecycleViewDivider(
    mContext, LinearLayoutManager.VERTICAL, R.drawable.divider_mileage));

3. 添加自定义分割线：可自定义分割线高度和颜色
mRecyclerView.addItemDecoration(new RecycleViewDivider(
    mContext, LinearLayoutManager.VERTICAL, 10, getResources().getColor(R.color.divide_gray_color)));
```
类实现
```java
package com.neso.settings;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.Rect;
import android.graphics.drawable.Drawable;
import android.support.v4.content.ContextCompat;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.View;

public class RecycleViewDivider extends RecyclerView.ItemDecoration {

    private Paint mPaint;
    private Drawable mDivider;
    // dividing line height,default 1px
    // 分割线高度，默认为 1px
    private int mDividerHeight = 2;
    // list orientation
    // 列表的方向：LinearLayoutManager.VERTICAL 或 LinearLayoutManager.HORIZONTAL
    private int mOrientation;
    private static final int[] ATTRS = new int[]{android.R.attr.listDivider};

    /**
     * default dividing line,height 2px,color is gray
     * 默认分割线：高度为 2px，颜色为灰色
     *
     * @param context
     * @param orientation
     */
    public RecycleViewDivider(Context context, int orientation) {
        if (orientation != LinearLayoutManager.VERTICAL && orientation != LinearLayoutManager.HORIZONTAL) {
            throw new IllegalArgumentException("Invalid orientation. It should be either HORIZONTAL or VERTICAL");
        }
        mOrientation = orientation;

        final TypedArray a = context.obtainStyledAttributes(ATTRS);
        mDivider = a.getDrawable(0);
        a.recycle();
    }

    /**
     * custom dividing line
     * 自定义分割线
     *
     * @param context
     * @param orientation
     * @param drawableId
     */
    public RecycleViewDivider(Context context, int orientation, int drawableId) {
        this(context, orientation);
        mDivider = ContextCompat.getDrawable(context, drawableId);
//        mDividerHeight = mDivider.getIntrinsicHeight();
    }

    /**
     * custom dividing line
     * 自定义分割线
     *
     * @param context
     * @param orientation
     * @param dividerHeight
     * @param dividerColor
     */
    public RecycleViewDivider(Context context, int orientation, int dividerHeight, int dividerColor) {
        this(context, orientation);
        mDividerHeight = dividerHeight;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(dividerColor);
        mPaint.setStyle(Paint.Style.FILL);
    }


    // get item offset
    // 获取 item 偏移量
    @Override
    public void getItemOffsets(Rect outRect, View view, RecyclerView parent, RecyclerView.State state) {
        super.getItemOffsets(outRect, view, parent, state);
        outRect.set(0, 0, 0, mDividerHeight);
    }

    // draw dividing line
    // 绘制分割线
    @Override
    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
        super.onDraw(c, parent, state);
        if (mOrientation == LinearLayoutManager.VERTICAL) {
            drawVertical(c, parent);
        } else {
            drawHorizontal(c, parent);
        }
    }

    // draw horizontal dividing line
    // 绘制横向 item 分割线
    private void drawHorizontal(Canvas canvas, RecyclerView parent) {
        final int left = parent.getPaddingLeft();
        final int right = parent.getMeasuredWidth() - parent.getPaddingRight();
        final int childSize = parent.getChildCount();
        for (int i = 0; i < childSize; i++) {
            final View child = parent.getChildAt(i);
            RecyclerView.LayoutParams layoutParams = (RecyclerView.LayoutParams) child.getLayoutParams();
            final int top = child.getBottom() + layoutParams.bottomMargin;
            final int bottom = top + mDividerHeight;
            if (mDivider != null) {
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(canvas);
            }
            if (mPaint != null) {
                canvas.drawRect(left, top, right, bottom, mPaint);
            }
        }
    }

    // draw vertical dividing line
    // 绘制纵向 item 分割线
    private void drawVertical(Canvas canvas, RecyclerView parent) {
        final int top = parent.getPaddingTop();
        final int bottom = parent.getMeasuredHeight() - parent.getPaddingBottom();
        final int childSize = parent.getChildCount();
        for (int i = 0; i < childSize; i++) {
            final View child = parent.getChildAt(i);
            RecyclerView.LayoutParams layoutParams = (RecyclerView.LayoutParams) child.getLayoutParams();
            final int left = child.getRight() + layoutParams.rightMargin;
            final int right = left + mDividerHeight;
            if (mDivider != null) {
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(canvas);
            }
            if (mPaint != null) {
                canvas.drawRect(left, top, right, bottom, mPaint);
            }
        }
    }
}
```
自定义的 drawable 文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <size android:height="20dp" />
    <solid android:color="#ff992900" />
</shape>
```

## 参考
- [RecyclerView 设置分隔线的三种方法](https://blog.csdn.net/yancychas/article/details/77484659)
- [Android RecyclerView 使用完全解析 体验艺术般的控件](https://blog.csdn.net/lmj623565791/article/details/45059587)