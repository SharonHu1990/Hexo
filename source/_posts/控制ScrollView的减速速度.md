title: 控制UIScrollView的减速速度
date: 2017-04-20 15:54:57
keywords: ScrollView
tags: [UIScrollView]
categories: iOS
---

手指在UIScrollView上滑动后，会再减速一段距离，如果觉得减速之后滑动的距离太远了，可以通过`decelerationRate`的值来控制减速的距离。

## 通过系统默认值修改
系统提供以下两个值：
UIScrollViewDecelerationRateNormal :正常减速
UIScrollViewDecelerationRateFast：快速减速
默认情况下UIScrollView使用UIScrollViewDecelerationRateNormal，如果将其改为UIScrollViewDecelerationRateFast，会发现滚动的**距离**明显降下来了。

    self.tableView.decelerationRate = UIScrollViewDecelerationRateFast；

## 通过自定义值修改

decelerationRate类型为CGFloat，范围是（0.0，1.0）。
上面两个常量的值分别是：
UIScrollViewDecelerationRateNormal :0.998
UIScrollViewDecelerationRateFast：0.99

如果以上值还不能满足需求的话，我们可以将其设为范围内的任意值。比如将其设置为0.1，会发现滑动之后很快就停下来了。
