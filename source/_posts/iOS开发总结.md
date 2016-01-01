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



### 使用%运算符出现错误：invalid operands to binary expression (‘CGFloat’(aka 'double') and 'CGFloat')

CGFloat c = a % b;
解决：% is for int or long, not float or double.
You can use fmod() or fmodf() from <math.h> instead.
Better is <tgmath.h> as suggested by the inventor of CGFloat.


### Objective-C浮点数向上取整和向下取整：
向上取整：ceil（f）//f为浮点数
向下取整：floor(f)//f为浮点数

    
### TableView的分割线从顶端开始：
        if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {  
          [self.tableView setLayoutMargins:UIEdgeInsetsZero];  
        } 
        
        
        if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {  
         [cell setSeparatorInset:UIEdgeInsetsZero];  
        }  
        if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {  
        [cell setLayoutMargins:UIEdgeInsetsZero];  
        }


### Swift中宏替换与代码标识
[看这里](http://www.hmttommy.com/2015/02/26/Swift%E4%BB%8E%E9%9B%B6%E5%8D%95%E6%8E%92%E7%B3%BB%E5%88%97-%E4%B8%80-%E4%B9%8B%E5%AE%8F%E6%9B%BF%E6%8D%A2%E4%B8%8E%E4%BB%A3%E7%A0%81%E6%A0%87%E8%AF%86/)


### 监听输入框内容的改变

    [textField addTarget:self action:@selector(textFieldValueChanged:) forControlEvents:UIControlEventEditingChanged];


### [button setTitleColor:[UIColor redColor] forState:UIControlStateSelected]不起作用

因为设置了button的AttributedTitle，所以在[button setSelected:YES];之前，先设AttributedTitle为nil
如下：
[button setAttributedTitle:nil forState:UIControlStateNormal];


### 保存图片到相册

    /**
     *  保存图片
     */
    - (void)saveImage{
        /**
         *  将图片保存到iPhone本地相册
         *  UIImage *image            图片对象
         *  id completionTarget       响应方法对象
         *  SEL completionSelector    方法
         *  void *contextInfo
         */
    
        UIImageWriteToSavedPhotosAlbum(_allImagesOfThisArticle[currentPhotoIndex], self, @selector(image:didFinishSavingWithError:contextInfo:), nil);
    }
    
    
    - (void)image:(UIImage *)image didFinishSavingWithError:(NSError *)error contextInfo: (void *)contextInfo
    {
        if (!error)
        {
            //it worked do the thing
        }
        
    }
    
    
### Request failed: unacceptable content-type: text/html using AFNetworking 2.0

解决方法是添加：
manager.responseSerializer.acceptableContentTypes = [manager.responseSerializer.acceptableContentTypes setByAddingObject:@"text/html"];


### Xcode打开代码聚焦栏(Focus Ribbon)的方式
![此处输入图片的描述][5]


### Xcode查找快捷方式（以code folding为例）
![此处输入图片的描述][6]

### 区分StoryBoard中的Show、Show Detail、Present Modally、Popover Presentation、Custom

- Show
    
将目标试图控制器push到navigation堆栈中，由右至左地显示试图控制器，提供一个后退按钮在navigation bar 上。


例如：应用Mail中导航到收件箱

 - Show Detail
 
在UISplitViewController中，替换详情视图或次级视图，不提供返回按钮。


例如：iPad上的邮箱应用在横屏时，点击左侧侧边栏的邮件列表，右侧显示邮件的详情。


 - Present Modally

通过定义不同的Presendation option，以不同的方式展示视图控制器，覆盖上一个视图控制器。最常用的呈现动画是从屏幕底部出现视图控制器，覆盖整个屏幕。但是，在iPad上，最常见的呈现方式是将视图控制器呈现在屏幕的正中间，并且将底部的视图控制器变暗，从下向上出现。


例如：点击iPhone上日历应用的"+"按钮。

 - Popover（iOS8之后改为：Present as Popover）
    
当在iPad中运行时，目标视图展示在一个小popover视图中，点击popover视图之外的其他任何地方，视图将消失。iPhone也支持popover，但是，如果连接一个Popover Presentation segue,将会与执行Present Modally的效果相同。


例如：在iPad上的日历应用中点击+按钮。


 - Custom
 
你可能会实现和控制自定义的视图切换方式。

** 这些视图切换类型在iOS8中被弃用：Push、Modal、Popover、Replace。**
 
[看这里，了解更多][7]。


### 关于M_PI
     #define M_PI     3.14159265358979323846264338327950288 
其实它就是圆周率的值，在这里代表弧度，相当于角度制 0-360 度，M_PI=180度 
旋转方向为：顺时针旋转 


    sender.transform = CGAffineTransformRotate(sender.transform, -M_PI_2*1.0);
    
    sender.transform = CGAffineTransformRotate(sender.transform, -M_PI_2*1.0);
            
            


 
 ### 判断ScrollView向左滑还是向右滑？

    -(void)scrollViewDidScroll:(UIScrollView *)scrollView
    {
    
            contentScrollViewCurrentPosition = scrollView.contentOffset;
            
                   
            if (contentScrollViewCurrentPosition.x < contentScrollViewStartPosition.x &&
                contentScrollViewCurrentPosition.x < myContentScrollView.contentSize.width) {
                scrollDirection = DirectionLeft;
            }else if(contentScrollViewCurrentPosition.x > contentScrollViewStartPosition.x &&
                     contentScrollViewCurrentPosition.x > 0){
                scrollDirection = DirectionRight;
            }
    }
    
    // 当开始滚动视图时，执行该方法。一次有效滑动（开始滑动，滑动一小段距离，只要手指不松开，只算一次滑动），只执行一次。
    - (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView{
        
    contentScrollViewStartPosition = scrollView.contentOffset;
    
    }





  [1]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-12%20%E4%B8%8A%E5%8D%889.35.57.png
  [2]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-14%20%E4%B8%8A%E5%8D%8810.38.35.png
  [3]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-15%20%E4%B8%8A%E5%8D%889.06.43.png
  [4]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-15%20%E4%B8%8A%E5%8D%889.10.01.png
  [5]: http://7xlt6k.com1.z0.glb.clouddn.com/%E6%89%93%E5%BC%80Xcode%E7%9A%84Focus%20Ribbon%E7%9A%84%E6%96%B9%E5%BC%8F.png
  [6]: http://7xlt6k.com1.z0.glb.clouddn.com/%E6%9F%A5%E6%89%BECode_Folding%E7%9B%B8%E5%85%B3%E7%9A%84%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F.png
  [7]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_storyboard/Chapters/StoryboardSegue.html