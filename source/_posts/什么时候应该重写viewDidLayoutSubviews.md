title: 什么时候应该重写viewDidLayoutSubviews?
toc: true
date: 2016-03-23 10:59:38
tags: iOS
categories: iOS
---
## viewDidLoad
重写viewDidLoad是为了配置任何你没有在XIB或者Storyboard中配置的东西。
当View controller的视图层从XIB或Storyboard被加载到内存中的时候，viewDidLoad也会被调用。
当在loadView方法中代码实现一个view的时候（使用loadVIew的时候，没有必要再使用viewDidLoad）
<!-- more -->
当ViewDidLoad被调用的时候，IBQutlets已经被连接，但是View还没有被加载出来，所以**可以在viewDidLoad中完成在IB中不能完成的view的自定义**。

注意：当viewController在navigation堆栈中，从此界面跳转到其他界面，再返回过来，不会再走ViewDidLoad方法，所以**不能把需要在viewController准备变为活跃状态的时候做相应更新的代码放在这里**。


## viewWillAppear
这个方法用来告诉view controller准备把View展示在屏幕上。
重写此方法以执行与应用程序的状态有关的设置以及将要展示在view中的数据。
实例：
- 用于controller使用的数据值
- UI定制，例如自定义依赖于数据的颜色或文字
- controller的状态等

## viewDidLayoutSubviews
这个方法在controller的的子视图的position和size被改变的时候被调用。
在view 已经被设计好了它的subviews并且还没有被展示在屏幕上时候，可以调用此方法改变这个view。
关键点是改变边界。任何依赖于bounds，并且需要去完成的操作都应该放在viewDidLayoutSubviews中，而不是viewDidLoad或viewWillAppear中。
因为view的frame和bounds直到Auto Layout 已经完成工作的时候才会被确定，所以在执行完Auto Layout之后会调用此方法。