title: UITableView section header 不固定
toc: true
date: 2016-07-09 21:01:31
tags: [iOS开发技巧]
categories: [iOS]
---
iOS系统自带的UITableView，当数据分为多个section的时候，在UITableView滑动的过程中，默认section header是固定在顶部的，滑动到下一个section的时候，下一个section header把上一个section header顶出屏幕外。典型的应用就是通讯录。

默认情况下，UITableView的section header是固定的，如何让section header不固定呢？也就是随着UITableView的滑动而滑动，顶部不是一直都显示section header。方法是设置UITableView 的contentInset。代码如下：

    #pragma mark - scrollView代理函数
    - (void)scrollViewDidScroll:(UIScrollView *)scrollView
    {
        // 修改contentSize
        if([scrollView isKindOfClass:[UITableView class]]){// 不固定section
            CGFloat sectionHeaderHeight = pxToCoordinate(TABLECELL_SECTION_HEADER);
            if (scrollView.contentOffset.y<=sectionHeaderHeight&&scrollView.contentOffset.y>=0) {
                scrollView.contentInset = UIEdgeInsetsMake(-scrollView.contentOffset.y, 0, 0, 0);
        } else if (scrollView.contentOffset.y>=sectionHeaderHeight) {
                scrollView.contentInset = UIEdgeInsetsMake(-sectionHeaderHeight, 0, 0, 0);
            }
        }
    }

原理：当滑动的值大于section header的高度时，设置其contentInset,达到不显示header的效果，这样就类似于将header给顶出了屏幕；当滑动至小于section header的高度时，恢复contentInset，显示header。


需要注意：因为UITableView 滑动时contentOffset会不断的改变，因此该部分代码需要写到 scrollViewDidScroll 方法中。