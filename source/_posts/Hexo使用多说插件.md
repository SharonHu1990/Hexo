title: Hexo使用多说插件
date: 2015-09-23 11:37:21
tags: [知识小集,Hexo]
toc: true
---

多说是一款追求极致体验的社会化评论框，可以用微博、QQ、人人、豆瓣等帐号登录并评论。下面简单说说如何在Hexo的博客中使用多说。

<!-- more -->

## 创建多说账号
打开[这个网页](http://duoshuo.com/create-site)创建。
![创建账号][1]
填入个人信息，点击创建。
其中 多说域名 填入的信息就是short_name，在后面要用到。


## 修改主题的_config.yml
在_config.yml中添加多说的配置，如下：

    duoshuo_shortname: 你站点的short_name



## 复制通用代码保存到博客模板
 ![此处输入图片的描述][2]
将通用代码中的：
请将此处替换成文章在你的站点中的ID 替换为 <%= page.path %>
请替换成文章的标题 替换为 <%= page.title %>
请替换成文章的网址 替换为 <%= page.permalink %>
效果如下：

    <div class="ds-thread" data-thread-key="<%= page.path %>" data-title="<%= page.title %>" data-url="<%= page.permalink %>"></div>

  [1]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-09-23%20%E4%B8%8A%E5%8D%8811.40.36.png
  [2]: http://7xlt6k.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202015-09-23%20%E4%B8%8A%E5%8D%8811.43.07.png
  
## 修改themes\landscape\layout\_partial\article.ejs模板

把

      <% if (!index && post.comments && config.disqus_shortname){ %>
      <section id="comments">
        <div id="disqus_thread">
          <noscript>Please enable JavaScript to view the <a href="//disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
        </div>
      </section>
      <% } %>
改为你复制的通用代码。