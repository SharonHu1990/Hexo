title: 新闻App优化：数据缓存
toc: true
date: 2015-12-22 09:29:39
tags: [iOS, 数据缓存]
categories: iOS
---
## 原因
应用需要离线工作的主要原因是：改善应用所表现出来的性能。将应用的内容缓存起来就可以支持离线。

<!-- more -->

## 思路：


### 创建一个AppCache单例类
创建一个单例类AppCache，用来管理缓存（包括内存缓存和磁盘缓存）。

### 缓存目录的创建：

- 在library/caches目录下，创建一个名为『MyAppCaches』的目录，在这个目录下，针对不同的用户，需要创建对应的缓存目录，目录名为用户的ID。比如，用户ID为12345，那么该用户的缓存目录为library/caches/MyAppCaches/12345。这样，在不同的用户目录下，就可以实现个性化的内容缓存。

- 首页列表缓存目录：
    创建首页列表缓存目录：library/caches/MyAppCaches/用户ID/HomeCaches
首页有多个不同的栏目，针对不同的栏目，创建不同的缓存目录，比如栏目『热门』的缓存目录为： library/caches/MyAppCaches/用户ID/HomeCaches/热门

- 新闻详情的缓存目录
    新闻详情页的缓存，统一保存在与首页列表缓存目录并列的详情页缓存目录中，如：library/caches/MyAppCaches/用户ID/NewsDetailCaches

### 实现首页列表数据的磁盘缓存
（以『推荐新闻』列表为例，其他栏目是一样的）

- 为新闻数据创建数据模型类，如NewsModel类，这个类需要遵循NSCoding协议，只有遵循了NSCoding协议，才能使用NSKeyedArchiver对数据模型实例进行归档和反归档。
- 在NewsModel类中，实现NSCoding协议方法：
- (id)initWithCoder:(NSCoder *)aDecoder
- - (void)encodeWithCoder:(NSCoder *)aCoder
- 使用NSKeyedArchiver/NSKeyedUnarchiver对数据模型进行归档和反归档

    归档：[NSKeyedArchiver archiveRootObject:objectForArchiving toFile:archiveFilePath];
    反归档：[NSKeyedArchiver archivedDataWithRootObject:objectForArchiving];

### 内存缓存（速度更快）

#### 创建内存缓存

- NSMutableDictionary类型的变量 cachedDirectory :保存缓存的数据
- NSMutableArray类型的变量 recentData：保存最近访问的数据
- 一个整数maxNumber：限制最大内存缓存大小

#### 保存到内存缓存

- 判断recentData数组中有没有这条数据
- 如果有相同数据，则删除这条数据，把数据插入到recentData的第一个位置
- 如果没有，直接插入到最后一个位置

#### 超过保存上限的处理机制
**当最近访问数据即recentData的count达到上限值maxNumber时，根据LRU（Least Recently Used，最近最少使用）算法做以下处理：**

- 从recentData中删除最后一个数据
- 从cachedDirectory中删除这个数据
- 将数据保存到磁盘中

#### 读取内存缓存

- 根据fileName（新闻ID ）查询  cachedDirectory中是否有数据
- 如果有，就返回cachedDirectory中的这条数据
- 如果没有内存缓存数据，则从磁盘中读取（根据filename（新闻ID）和cachePath 路径）

### 添加对内存警告的处理机制

- 在AppCache但单例类的静态初始化方法中，注册一个对内存警告通知UIApplicationDidReceiveMemoryWarningNotification的监听器。
- 在接收到内存警告时，将内存缓存中的数据保存到磁盘中。

### 添加对App退出和进入后台时的内存管理机制

- 在AppCache但单例类的静态初始化方法中，注册一个对App进入后台的监听器：UIApplicationDidEnterBackgroundNotification
- 在AppCache但单例类的静态初始化方法中，注册一个对App退出的监听器：UIApplicationWillTerminateNotification
- 在接收到上面两个监听器的通知后，将内存缓存中的数据保存到磁盘。


## 参考资料
人民邮电出版社《iOS6编程实战》第二十四章『离线支持』

