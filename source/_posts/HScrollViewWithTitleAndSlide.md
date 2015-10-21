﻿title: HScrollViewWithTitleAndSlide
toc: true
date: 2015-10-20 17:13:11
tags: [iOS, Demo]
categories: [iOS]
---

## 功能说明
1. 模仿LOFTER发现界面的页面切换效果
2. 标题可以随着内容的滚动而滚动
3. 下拉展示所有标题以供点选
2. 下拉按钮的图片和勾选的图片可以自定义，每页最多显示的标题的个数可以自定义。
3. 封装的比较完整，使用起来很简单，几句代码搞定。
4. 使用Xcode7.0.1  Objective-C
5. GitHub地址：https://github.com/SharonHu1990/HScrollViewWithTitleAndSlide

<!-- more -->

## 框架使用说明


1. 拖拽HSlideScrollView文件夹到你的工程目录.
2. 在需要使用该框架的ViewController中添加如下代码：

代码示例：

    /**
     *  添加MySlideScrollView
     */
    -(void)addMySlideScrollView
    {
        CGRect slideScrollFrame = CGRectMake(0, 64, self.view.frame.size.width, self.view.frame.size.height-64);
        NSArray *titlesArray = [[NSArray alloc] initWithObjects:@"A", @"B", @"C", @"D", @"E", @"F", @"G", @"H", @"I", @"J",     @"K", @"L", @"M", @"N", nil];
        mySlideScrollView = [[HSlideScrollView alloc] initWithFrame:slideScrollFrame andTitleArrays:titlesArray     andTitleScrollerViewHight:40.f andNumverOfTitlesPerPage:7 andArrowImage:[UIImage imageNamed:@"arrow_down"] andTickImage:[UIImage         imageNamed:@"tick"] andTitleListTitle:@"分类"];
         [self.view addSubview:mySlideScrollView];
    }


## 功能演示
![功能演示](http://7xlt6k.com1.z0.glb.clouddn.com/SlideScrollView.gif)






