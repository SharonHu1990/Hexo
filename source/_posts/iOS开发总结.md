title: iOS开发总结
toc: true
date: 2015-10-15 08:53:04
tags: [知识小集, iOS]
categories: [iOS]
---

记录iOS开发过程中使用的技术、遇到的问题以及解决方法。

<!-- more -->

### 自定义系统键盘上方的View有两种方法
1.设定textfield或者textview的inputAccessoryView属性
2.监听键盘事件，获取键盘高度，动态改变自定义View的位置


### CocoaPods 执行 pod update 和 pod install 卡住不动
使用加参数的命令：
pod install --verbose --no-repo-update
或者 
pod update --verbose --no-repo-update



### 添加PCH文件
![添加PCH文件][1]
注意在BuildSetting中设置Prefix header路径




### 添加讯飞语音需要的系统类库
![添加讯飞语音需要注意的][2]

### 安装CocoaPods错误
如果是下面这个错误：
Could not find a valid gem 'cocoapods' (>= 0), here is why: Unable to download data from https://rubygems.org/ - Errno::ETIMEDOUT: Op...

解决方法，[看这里!](http://stackoverflow.com/questions/15305350/gem-install-fails-with-openssl-failure)



### Undefined symbols for architecture arm64
1. 检查Pod的TARGETS和工程项目的TARGETS的BuildSetting
![此处输入图片的描述][3]

2. 检查工程项目TARGETS的Other Linder Flags 
![此处输入图片的描述][4]



### 理解armv7 armv7s arm64 i386 x86_64

[看这篇博文](http://blog.csdn.net/lizhongfu2013/article/details/42387311)



### 什么时候用Block或代理？
[看这篇博文](http://blog.cocoabit.com/2014-01-19-block-delegate/)
1.要是一个对象有超过一个的不同的事件，使用代理
2.要是一个对象是个单例，我们不能使用代理
3.要是一个对象调用方法需要返回一些额外的信息，我们可能需要使用代理
4.过程 vs. 结果
5.速度（也许吧？）

具体情况，具体分析!



### 
    
    
  [1]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-12%20%E4%B8%8A%E5%8D%889.35.57.png
  [2]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-14%20%E4%B8%8A%E5%8D%8810.38.35.png
  [3]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-15%20%E4%B8%8A%E5%8D%889.06.43.png
  [4]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-15%20%E4%B8%8A%E5%8D%889.10.01.png