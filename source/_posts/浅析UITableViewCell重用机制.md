title: 浅析UITableViewCell重用机制
toc: true
date: 2015-10-28 10:31:02
tags: [iOS, UITableView, 知识小集]
categories: [iOS]
---
UITableView在iOS开发中用的非常的多，由于Cell中一般都会有Image等占用内存的资源，容易引起Memory Warning，所以iOS引入了重用机制。

# 案例分析

## 情况A：所有Cell具有相同的类型

    -(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
    {
        static NSInteger times = 0;
        static NSString *cellIdentifier = @"Default Type";
        UITableViewCell *myCell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
        if (myCell == nil) {
            myCell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
            NSLog(@"创建%d次",++times);
        }
        myCell.textLabel.text= [NSString stringWithFormat:@"第%d行 %@",indexPath.row+1, cellIdentifier];
        return myCell;
    }
    
分析：

- 有两个存放Cell的队列：可复用Cell队列 **reusableCellQueue** 和可见的Cell队列 **visualCellQueue**；
- 执行`cellForRowAtIndexPath`之前，先从reusableCellQueue中寻找标识为`Default Style`的Cell，如果没有，返回`nil`，接着会执行`initWithStyle：reuseIdentifier`；
- 假设屏幕显示 **11行** Cell，如果不滚动TableView，reusableCellQueue是空的，Cell被创建了11次；
- 向上拖动TableView，使第12行Cell出现在屏幕中（加入到visualCellQueue中），这时，reusableCellQueue仍然是空的。所以又创建了一次Cell；
- 当第12行Cell完全出现在visualCellQueue中，第1行Cell就加入到了reusableCellQueue中。
- 再次向上拖动TableView，使第13行Cell出现。注意，这时从reusableCellQueue中寻找到了标识为`Default Style`的Cell，于是第一行Cell被复用，不用重新创建Cell。
- 以后再上下滑动，都会在reusableCellQueue中找到可复用的Cell，因此，此TableView完成完整的滚动需要创建 **12次** Cell。
- 总结：第一页显示N行Cell，则一共创建了N+1次。
    
## 情况B：具有多种类型的Cell

        -(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
    {
        NSString *cellIdentifier;
        UITableViewCellStyle cellStyle;
        switch ((indexPath.row + 1)%4) {
            case 1:{
                //有标题和副标题，可选图片
                cellIdentifier = @"Subtitle Style";
                cellStyle = UITableViewCellStyleSubtitle;
            }
                break;
            case 2:{
                //左边文字左对齐，右边文字右对齐，可选的图片
                cellIdentifier = @"Value1 Style";
                cellStyle = UITableViewCellStyleValue1;
            }
                break;
            case 3:{
                //左边文字右对齐，蓝色字体。右边文字左对齐，黑色。没有图片
                cellIdentifier = @"Value2 Style";
                cellStyle = UITableViewCellStyleValue2;
            }
                break;
            default:{
                //有标题，没有副标题，可选的图片
                cellIdentifier = @"Default Style";
                cellStyle = UITableViewCellStyleDefault;
            }
                break;
        }
    
        static NSInteger times = 0;
        UITableViewCell *myCell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
        if (!myCell) {
            myCell = [[UITableViewCell alloc] initWithStyle:cellStyle reuseIdentifier:cellIdentifier];
            NSLog(@"创建%d次",++times);
        }
        myCell.textLabel.text = [NSString stringWithFormat:@"第%d行%@",indexPath.row+1, cellIdentifier];
        myCell.detailTextLabel.text = @"Subtitle Text";
        if (indexPath.row > 3) {
            myCell.imageView.image = [UIImage imageNamed:@"smile.png"];
        }
        return myCell;
    }
    
    
运行结果是这样的：
    ![不同ReusableCellIdentifier的Cell][1]

### 分析：

-  第一页显示11个Cell，创建了11次。
-  向上拖动TableView，使第12行Cell出现，第12行是`Default Style`类型的，可复用队列为空，没有找到可复用Cell，于是又创建一次（第12次）。第一个Cell进入reusableCellQueue中。此时reusableCellQueue中只有一个`Subtitle Style`的Cell。
-  再次向上拖动，当第13个Cell出现时，从reusableCellQueue中寻找标识为`Subtitle Style`的Cell，Yes，reusableCellQueue里有这个标识的Cell，于是复用队列里的这个Cell。此时第二个Cell进入reusableCellQueue，队列里有这几个标识：`Subtitle Style`和`Value1 Style`。
-  当第14个Cell出现时，寻找标识为`Value1 Style`的Cell，也找到了，复用之。此时队列里有`Subtitle Style`、`Value1 Style`和`Value2 Style`
-  以此类推，以后的Cell都可以在reusableCellQueue中找到可复用的Cell。因此一共创建了12次Cell。

### 不该有图片的Cell出现了图片
当上下滑动TableView的时候，会出现第一行的Cell一会有图片，一会儿又没有图片的现象。这是为什么呢？


第一个Cell应该是没有图片的，但是在TableView向下滚动，使第一个Cell出现在屏幕上的时候，会先从reusableCellQueue中寻找标识为`Subtitle Style`的Cell。注意了，第5、9、13、17行的Cell都是Subtitle Style类型的，而且还都带有图片，因此，当这些类型的Cell在reusableCellQueue中被寻找到时，第一行Cell上就会出现图片。


那么，如何解决这类问题呢？


在配置Cell的时候一定要注意，对取出的重用的cell要重新赋值，不能遗留被重用Cell的数据。

    

# 区分两个获取重用Cell的方法

## - dequeueReusableCellWithIdentifier:forIndexPath:
此方法返回一个相关标识的UITableViewCell对象，这个Cell总是有效的(不是nil)。

** 注意：**
使用这个方法之前，必须使用`registerNib:forCellReuseIdentifier:`或者`registerClass:forCellReuseIdentifier:`注册一个Cell类或者nib。


    [tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:cellIdentifier];
        UITableViewCell *myCell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier forIndexPath:indexPath];

## - dequeueReusableCellWithIdentifier:
   返回值 : 相关标识的UITableViewCell对象，或者是nil(如果在可重用Cell队列中没有找到的话)。
   
       UITableViewCell *myCell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    if (myCell == nil) {
        myCell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
    }
## 比较
    使用`- dequeueReusableCellWithIdentifier:forIndexPath:`的话，必须注册Cell，而且，不需要再判断Cell是否为nil和创建 Cell。
    
    



    
    
    


  [1]: http://7xlt6k.com1.z0.glb.clouddn.com/Simulator%20Screen%20Shot%202015%E5%B9%B410%E6%9C%8828%E6%97%A5%20%E4%B8%8B%E5%8D%881.41.28.png