title: 使用网络代理造成Archive submission failed with errors
toc: false
date: 2015-10-09 10:02:47
tags: [iOS,Archive Submission, 知识小集]
categories: iOS
---
提交archive到AppStore，出现这个错误：
![截图][1]
<!-- more -->

从中可以看出是网络原因导致的，我使用了鱼摆摆网络代理，所以有可能是因为使用了鱼摆摆。


那么如何解决呢？


首先，打开鱼摆摆的设置，点击“高级”，查看是否勾选了代理所有网站，如果勾选了，取消掉。
![截图][2]


然后，取消启用系统代理（不选第一行的“启用系统代理”）。
![截图][3]


做完上述的设置后，重新提交archive，成功。
![此处输入图片的描述][4]


  [1]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-09%20%E4%B8%8A%E5%8D%889.11.31.png
  [2]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-09%20%E4%B8%8A%E5%8D%889.25.40.png
  [3]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-09%20%E4%B8%8A%E5%8D%889.25.27.jpg
  [4]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-10-09%20%E4%B8%8A%E5%8D%889.25.21.png