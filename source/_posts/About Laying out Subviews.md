title: About Laying out Subviews
toc: true
date: 2016-04-11 16:30:39
tags: [知识小集, iOS]
categories: iOS
---
## layoutSubviews

官方文档引用：

> You should override this method only if the autoresizing and
> constraint-based behaviors of the subviews do not offer the behavior
> you want.

    
我的理解是：
    在subview的`autoresizing`和`constraint`不满足你的要求时，可以重写`layoutSubviews`。假设一个屏幕从竖屏变为横屏，本来图文单元是图片在上，文字在下面，可能就变为了左右结构；这是无论自动适配还是自动布局都解决不了，就只能重写layoutSubviews函数了。

<!-- more -->

官方文档引用:

> You should not call this method directly.

不应该直接调用这个方法！

官方不建议我们直接调用`layoutSubviews`，其中的具体原因我想的不是特别明白，但是，我觉得可以换个角度来思考这个问题，来想想什么时候layoutSubviews会被自动调用，可能会对理解官方不建议直接调用`layoutSubviews`有帮助。

`layoutSubviews`在以下情况下会被调用：
 - `init`初始化不会触发`layoutSubviews`,但是使用`initWithFrame` 进行初始化时，当rect的值不为`CGRectZero`时,也会触发；
 - `addSubview`会触发`layoutSubviews`;
 - 设置view的Frame会触发`layoutSubviews`，当然前提是frame的值设置前后发生了变化;
 - 滚动一个`UIScrollView`及其派生类会触发`layoutSubviews`;
 - 旋转Screen会触发父`UIView`上的`layoutSubviews`事件
 - 改变一个`UIView`大小的时候也会触发父`UIView`上的`layoutSubviews`事件.


## setNeedsLayout

官方文档引用：

> Invalidates the current layout of the receiver and triggers a layout update during the next update cycle.
>     
Call this method on your application’s main thread when you want to adjust the layout of a view’s subviews. This method makes a note of
> the request and returns immediately. Because this method does not
> force an immediate update, but instead waits for the next update
> cycle, you can use it to invalidate the layout of multiple views
> before any of those views are updated. This behavior allows you to
> consolidate all of your layout updates to one update cycle, which is
> usually better for performance.

`setNeedsLayout`在UIView对象中设置一个标记，用来表示view需要被布局。这将使`layoutSubviews`方法在**下一次重绘发生之前**被异步调用。注意，因为UIView的`autoresizesSubviews`属性，在很多情况下，你不需要明确地调用`setNeedsLayout`，因为如果UIView的`autoresizesSubviews`被设置（默认被设置为YES），那么，当它的frame发生变化的时候就会自动去布局它的subViews。

此方法会将view当前的layout设置为无效的，并在下一个upadte cycle里去触发layout更新。

`setNeedsLayout`在以下情况下需要被调用：（帮助理解）

* If you manipulated constraints directly, call setNeedsLayout.
* If you changed some conditions (like offsets or smth) which would change constraints in your overridden `updateConstraints` method (a recommended way to change constraints, btw), call `setNeedsUpdateConstraints`, and most of the time, setNeedsLayout after that.
* If you need any of the actions above to have immediate effect—e.g. when your need to learn new frame height after a layout pass—append it with a layoutIfNeeded.

## layoutIfNeeded

官方解释：

> Lays out the subviews immediately.
> 
> Use this method to force the layout of subviews before drawing. Using
> the view that receives the message as the root view, this method lays
> out the view subtree starting at the root.

使用此方法强制立即进行layout,从当前view开始，此方法会遍历整个view层次(包括superviews)请求layout。因此，调用此方法会强制整个view层次布局。

如果要立即刷新，要先调用`[view setNeedsLayout]`，把标记设为需要布局，然后马上调用`[view layoutIfNeeded]`，实现布局
在视图第一次显示之前，标记总是“需要刷新”的，可以直接调用`[view layoutIfNeeded]`。

一个Demo:https://github.com/SharonHu1990/MyDemos/tree/master/LayoutSubViewsTest

在这个Demo中，一个UILabel被放在左上角，中间放一个button，点击button实现label左右移动，带有回弹动画。在xib中已经绑定了约束。

在button的action中，通过修改label的constraint.constant值实现位置的改变。代码如下：

    - (IBAction)touchButton:(id)sender {
        if (!_hasDistance) {
            _hasDistance = YES;
            _leftConstraint.constant = 100;
            
        }else{
            _hasDistance = NO;
            _leftConstraint.constant = 0;
        }
        
        [UIView animateWithDuration:0.8 delay:0 usingSpringWithDamping:0.5 initialSpringVelocity:0.5 options:UIViewAnimationOptionAllowAnimatedContent animations:^{
            [self.view layoutIfNeeded];//立即实现布局，如果不写这一句就没有动画效果
        } completion:nil];
    }

可以看到，在animations中写了`[self.view layoutIfNeeded]`,如果没有这句代码，label只是简单地移动了位置（即只是执行了_leftConstraint.constant = XX），而没有动画效果。



## setNeedsDisplay

官方解释：

> Marks the receiver’s entire bounds rectangle as needing to be redrawn.
> 
> You can use this method or the `setNeedsDisplayInRect:` to notify the
> system that your view’s contents need to be redrawn. This method makes
> a note of the request and returns immediately. The view is not
> actually redrawn until the next drawing cycle, at which point all
> invalidated views are updated.
> 
> You should use this method to request that a view be redrawn only when
> the content or appearance of the view change.

一个使用`setNeedsDisplay` 的例子:
http://blog.fujianjin6471.com/2015/06/11/An-example-of-when-should-setNeedsDisplay-be-called.html

`setNeedsLayout` 和 `setNeedsDisplay`的区别？
首先两个方法都是异步执行的。而setNeedsDisplay会自动调用`drawRect`方法，这样可以拿到`UIGraphicsGetCurrentContext`，就可以画画了。而`setNeedsLayout`会默认调用`layoutSubViews`，就可以处理子视图中的一些数据。
综上所述，`setNeedsDisplay`方便绘图，而`setNeedsLayout`方便出来数据。

## requiresConstraintBasedLayout

官方文档：

> A Boolean value that indicates whether the receiver depends on the
> constraint-based layout system.
> 
> Custom views should override this to return YES if they cannot layout
> correctly using autoresizing.


UIView.h中是这样说的(这个解释更加帮助了理解)：

> constraint-based layout engages lazily when someone tries to use it
> (e.g., adds a constraint to a view).  If you do all of your constraint
> set up in -updateConstraints, you might never even receive
> updateConstraints if no one makes a constraint.  To fix this chicken
> and egg problem, override this method to return YES if your view needs
> the window to use constraint-based layout.

意思就是：基于约束的布局是懒触发的(例如：给一个view添加约束)。如果把所有的约束放在 updateConstraints中，并且没有添加任何约束，那么你将不会获得updateConstraint。为了解决这个鸡和蛋的问题，重写+requiresConstraintBasedLayout 并且返回YES就是明确告诉系统：虽然我之前没有添加约束,但我确实是基于约束的布局！这样可以保证系统一定会调用 -updateConstraints 方法 从而正确添加约束.

一篇[objc.io][1] 中的文章也建议在实现使用了constraint的自定义View时，重写+requiresConstraintBasedLayout并返回YES，以声明view是依赖于AutoLayout的。

另外一篇参考文章：[The Mystery of the +requiresConstraintBasedLayout][2]


 
## translatesAutoresizingMaskIntoConstraints

在代码中创建view及其派生类，如果需要给他们添加约束，就将translatesAutoresizingMaskIntoConstraints 设为NO。原因：The reason for this is that iOS creates constraints for you that match the new view's size and position, and if you try to add your own constraints these will conflict and your app will break.

官方文档：

> A Boolean value that determines whether the view’s autoresizing mask
> is translated into Auto Layout constraints
> 
> If this property’s value is YES, the system creates a set of
> constraints that duplicate the behavior specified by the view’s
> autoresizing mask. This also lets you modify the view’s size and
> location using the view’s frame, bounds, or center properties,
> allowing you to create a static, frame-based layout within Auto
> Layout.
> 
> 
> Note that the autoresizing mask constraints fully specify the view’s
> size and position; therefore, you cannot add additional constraints to
> modify this size or position without introducing conflicts. If you
> want to use Auto Layout to dynamically calculate the size and position
> of your view, you must set this property to NO, and then provide a non
> ambiguous, nonconflicting set of constraints for the view.
> 
> 
> By default, the property is set to YES for any view you
> programmatically create. If you add views in Interface Builder, the
> system automatically sets this property to NO.


  [1]: https://www.objc.io/issues/3-views/advanced-auto-layout-toolbox/#local_constraints
  [2]: http://www.edwardhuynh.com/blog/2013/11/24/the-mystery-of-the-requiresconstraintbasedlayout/