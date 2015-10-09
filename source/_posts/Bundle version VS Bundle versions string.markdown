title: Bundle version VS Bundle versions string
date: 2015-09-20 14:14:23
tags: [知识小集,iOS,Xcode,版本号]
toc: true
category: iOS
---




> 今天上传新版本，在修改Bundle version和Bundle versions string的时候突然想到：一直以来都没有深究过这两个字段的真正含义，只是保持它们一样。那么它们真正的用途到底是什么呢？今天来探究一下。

<!-- more -->


## 区别
### Bundle Version (CFBundleVersion) 
- Bundle Version是应用程序的内部版本号。
- Bundle Version不需要是一个纯粹的版本号，它可以是1234，也可以是1.2.3(Build 12345AB)


### Bundle Version String (CFBundleShortVersionString) 
- Bundle versions string 是应用程序公开可见的版本号。例如，你每次迭代一个内部测试版本时，都会生成一个版本号，这个版本号可能是2.0.0.12345b7，但是你不想让其公开可见，所以你设置应用程序的短版本号为2.0。
- 必须与用于iTunes Connect的版本号保持一致。
- Bundle Version String不能超过三个部分。例如：2.0.1是可以的，但是2.0.0.1是不可以的。
- 当Bundle Version String缺省时，Bundle Version替代Bundle Version String的功能，同时也继承他的限制(比如格式，位数等)，展示在AppStore中。


##  Xcode 设置自增编译版本号
步骤：
1. Info.plist 中设置Bundle Version String；
2. Info.plist中Bundle version设置为数字 比如1，如果设置为其它，则可能会编译错误；
3. 添加脚本
    1. TARGETS -> Build Phases
    2. 点击左上角的‘+’，在弹出的选择框中点击New Run Script Phase，如下图：
    ![图1][1]
    3. 在Run script中添加以下脚本：
    

    version=`/usr/libexec/PlistBuddy -c "Print CFBundleVersion" $PRODUCT_SETTINGS_PATH`
    version=`expr $version + 1`
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $version" $PRODUCT_SETTINGS_PATH
    #/usr/libexec/PlistBuddy -c "Set :CFBundleShortVersionString $version" $PRODUCT_SETTINGS_PATH 这行代码会让version也自增，一般不需要

如图：
![图1][2]

  [1]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-09-21%20%E4%B8%8B%E5%8D%883.55.49.png
  [2]: http://7xlt6k.com1.z0.glb.clouddn.com/09211611.png
  

OK,这样设置以后，每次编译，Build version都会自增1.