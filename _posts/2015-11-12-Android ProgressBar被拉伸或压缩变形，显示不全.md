---
layout: post
title: Android ProgressBar被拉伸或压缩变形，显示不全
category: Android
tags: Android
keywords: ProgressBar,显示不全
description: Android ProgressBar被拉伸或压缩变形，显示不全
---

[TOC]

## 前奏
progressbar在xml的写法是：
```
<ProgressBar
            android:id="@+id/progressBar1"
            style="?android:attr/progressBarStyleHorizontal"
            android:layout_width="133.3dp"
            android:layout_height="42.4dp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:minHeight="42.4dp"
            android:minWidth="133.3dp"
            android:progressDrawable="@drawable/networkdata_down_book_progress_selector" />
```
 
networkdata_down_book_progress_selector的xml文件是：

```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" >

    <item android:id="@android:id/background"  android:drawable="@drawable/networkdata_btn_normal"></item>
    <item android:id="@android:id/progress" android:drawable="@drawable/networkdata_btn_press"></item>

</layer-list>
```

## 现象 
将图片networkdata_btn_normal和networkdata_btn_press放在drawable-mdpi，现象是

![这里写图片描述](http://img.blog.csdn.net/20151112113158114)

而把图片放在drawable-xhdpi，现象是

![这里写图片描述](http://img.blog.csdn.net/20151112113613090)

最终把图放在drawable-nodpi里，现象是

![这里写图片描述](http://img.blog.csdn.net/20151112161552954)


## WHY？ 
progressbar的android:progressDrawable属性是绘制draw图片，只有图的大小和progressbar大小一样才会正常显示，不然要么图片被截取显示，要么拉伸显示。


## 结论
**我用的机器屏幕密度是1.65，也就是264dpi。而mdpi是160dpi，hdpi是240dpi，xhdpi是320dpi，所以在drawable-mdpi和drawable-hdpi里，图片只显示一部分 ，在drawabe-xhdpi里会拉伸显示。 把图放在drawable-mdpi里，图会变成原来的宽高*（264/160），也就是放大了1.65倍，而progressbar设定宽高，那么就会从放大后的图里，以左上角为原点，截取progressbar的宽高大小的图，hdpi同理是放大，所以显示不全；而在xhdpi里，图会变成原来的宽高*（264/320），也就是缩小了0.825倍，那么要取progressbar的大小，也就需要填充，所以绘制第二张图来填满progressbar。把图放在drawable-nodpi，那么只会获取原图的大小，所以只要把progressbar的大小设为和图的大小一样就可以了。**

 
**PS：图片放在不同的资源文件夹里也会造成内存消耗不同，感兴趣的朋友可以参考：[ 关于Android中图片大小、内存占用与drawable文件夹关系的研究与分析](http://blog.csdn.net/zhaokaiqiang1992/article/details/49787117) **

**欢迎回复讨论！**