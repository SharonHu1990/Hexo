title: Markdown为写作而生
date: 2015-09-16 17:02:23
tags: [知识小集,Markdown,写作工具]
toc: true
---

> 学习Markdown知识，纵享写作之乐！
 
<!-- more -->

## 写作有什么难题？Markdown是如何解决的？


### .doc 或 Pages 格式有如下问题：

1. 不一定谁都能打开。用 Windows 的人打不开 .pages 文件，用旧版 Word 的人不一定能打开你用新版 Word 写的稿子。

2. 对方看到的稿子的样子和你自己看到的可能差别很大。

3. Office 已经是你电脑上唯一的盗版软件，导致心情不佳。


### 使用Markdown有如下好处：
1. **兼顾了「什么人都能打开」和「样式」**
    Markdown 就是纯文本，就是 txt，所以什么人都能打开。而如上所述，你可以用它来标记文本的样式，而且语法非常简单。

  由于是纯文本，Markdown 文稿也不会因为未来软件升级而产生不同版本之间的兼容问题，即，不会出现「我这篇稿子是用旧版 Word 写的，你用新版 Word 看可能格式会有点问题」的情况。
  
2. **Markdown 转 HTML 非常方便，对未来有益处**
    HTML 是整个万维网（web）的标记语言，但更重要的是，它也是目前主流电子书格式所用的标记语言。无论是 EPUB, mobi，还是 Kindle 用的专有格式 .azw，都只是把一堆 HTML 文件打包而已。如果你写的是书，用 Markdown 标注格式之后，可以很方便地转为以上格式（当然这个转换工作不需要由你来做）；如果你写的是单篇的文章（例如新闻报道或专栏），未来也不排除结集出书的可能。若采用 Markdown，对于日后的文件转换工作也大有裨益。


## Markdown的使用群

    
### 文艺青年

 - [為什麼文科生也該用markdown寫作](http://www.douban.com/note/221187015/)
 - [为什么作家应该用 Markdown 保存自己的文稿](http://www.jianshu.com/p/qqGjLN)

### 科学青年

 - [如何学习科学：开放科学工具箱](https://github.com/ouyangzhiping/openscience/blob/master/README.md)


## Markdown基本语法



### 段落


段落的前后要有一个以上的空行。（空行的定义是显示上看起来像是空行，就被视为空行。例如一行中只有空白和Tab，那么该行也被视为空行），普通的段落不需要用空白或制表符来缩进。


----------

### 标题


Markdown 支持两种标题的语法:

 - Setext:用底线的形式，利用 = （最高阶标题）和 - （第二阶标题）
 - atx:行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶

Setext:

    标题一
    =======
        
    标题二
    ------



atx：

        # 标题一
        ## 标题二
        ### 标题三
        #### 标题四
        ##### 标题五
        ###### 标题六



----------


### 区块引用


区块引用则使用 email 形式的 '>' 角括号

    >这是一个区块引用
    >
    >这是区块引用的第二段
    >
    >## 这是一个区块引用中的二级标题
    
显示效果为：
>这是一个区块引用
>
>这是区块引用的第二段
>
>## 这是一个区块引用中的二级标题


**输出HTML：**

    <h1>A first Leavel Header</h1>
    <h2>A Second Leavel Header</h2>
    <p>Now is the time for all good men to come to
    the aid of their country. This is just a
    regular paragraph.</p>
    <p>The quick brown fox jumped over the lazy
    dog's back.</p>
    <h3>Header 3</h3>
    <blockquote>
    <p>This is a blockquote.</p>
    <p>This is the second paragraph in the blockquote.</p>
    <h2>This is an H2 in a blockquote</h2>
    </blockquote>

----------


### 修辞和强调


Markdown 使用星号和底线来标记需要强调的区段。
示例：

     *文字两边各添加一个星号表示斜体字*
     _文字两边各添加一个短下划线表示斜体字_
    
    
    **文字两边各添加两个星号表示粗体字**
    __文字两边各添加两个短下划线表示粗体字__
    
显示效果为：
*文字两边各添加一个星号表示斜体字*
_文字两边各添加一个短下划线表示斜体字_
    
**文字两边各添加两个星号表示粗体字**
__文字两边各添加两个短下划线表示粗体字__

输出HTML为：

    <em>斜体字 </em>
    <strong>粗体字</strong>


----------

### 无序列表


无序列表使用星号、加号和减号来做为列表的项目标记，这些符号是都可以使用的。任何数量的 * 、= 和 - 都可以有效果。


使用星号：
星号前面再添加一个空格则生成一个二级列表。

     * Item1
     * Item2
      * 1
      * 2
      * 3
     * Item3


使用加号：

    + Item1
    + Item2
     + 1
     + 2
     + 3
    + Item3


使用减号：

    - Item1
    - Item2
     - 1
    - 2
    - 3
    - Item3

以上的显示效果都是这样的：

- Item1
- Item2
 - 1
 - 2
 - 3
- Item3

以上都会输出HTML为：

    <ul>
    <li>Item1</li>
    <li>Item2</li>
    <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    </ul>
    <li>Item3</li>
    </ul>


----------


### 有序列表


有序的列表则是使用一般的数字接着一个英文句点,后跟一个空格，作为项目标记

        1. Item1
        2. Item2
         1. Red
        2. Yellow
        3. Blue
        3. Item3

显示效果为：

1. Item1
2. Item2
    1. Red
    2. Yellow
    3. Blue
3. Item3


输出的HTML为：

    <ol>
    <li>Item1</li>
    <li>Item2</li>
    <ol>
    <li>Red</li>
    <li>Yellow</li>
    <li>Blue</li>
    </ol>
    <li>Item3</li>
    </ol>
    
   


----------



### 代码区块


和程序相关的写作或是标签语言原始码通常会有已经排版好的代码区块，通常这些区块我们并不希望它以一般段落文件的方式去排版，而是照原来的样子显示，Markdown 会用 `<pre>`和`<code>`标签来把代码区块包起来。

要在 Markdown 中建立代码区块很简单，只要简单地缩进 4 个空格或是 1 个制表符就可以，例如，下面的输入：

这是一个普通段落

    这是一个代码区块


Markdown 会转换成：

    <p>这是一个普通段落</p>
    
    <pre><code>这是一个代码区块</code></pre>
    

Here is an example of AppleScript:

    tell application "Foo"
    beep
    end tell

会被Markdown转化成：

    <p>Here is an example of AppleScript:</p>
    
    <pre><code>tell application "Foo"
        beep
    end tell
    </code></pre>
    
    
一个代码区块会一直持续到没有缩进的那一行（或是文件结尾）。

在代码区块里面， & 、 < 和 > 会自动转成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入范例用的 HTML 原始码，只需要复制贴上，再加上缩进就可以了，剩下的 Markdown 都会帮你处理，例如：

     <div class="footer">
            &copy; 2004 Foo Corporation
        </div>
        

会被Markdown转为：

    <pre><code>&lt;div class="footer"&gt;
        &amp;copy; 2004 Foo Corporation
    &lt;/div&gt;
    </code></pre>
    
  


----------


### 分割线


你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：

     * * *
     ***
     **********
     - - -
     ------------


----------


### 链接


Markdown 支持两种形式的链接语法： **行内式**和**参考式**两种形式。不管是哪一种，链接文字都是用 [方括号] 来标记。

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

这是一个[行内链接](http://example.com/ "链接的Title")的例子

这是一个没有title的[行内链接](http://example.com/)

MarkDown会产生：

    这是一个[行内链接](http://example.com/ "链接的Title")的例子
    
    这是一个没有title的[行内链接](http://example.com/)
    
    
如果是链接到同样主机的资源，可以使用相对路径：
See my [About](/about ) page for details
Markdown会产生：

    See my [About](/about ) page for details


参考式的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：
This is [an example] [id] reference-style link.
Markdown产生：

    This is [an example] [id] reference-style link.

接着，在文件的任意处，你可以把这个标记的链接内容定义出来：
    [id]:http://example.com/  "Optional Title Here"


链接内容定义的形式为:

 - 方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接标记文字
 - 接着一个冒号
 - 接着一个以上的空格或制表符
 - 接着链接的网址
 - 选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

 
下面这三种链接的定义都是相同：

    [foo]: http://example.com/  "Optional Title Here"
    [foo]: http://example.com/  'Optional Title Here'
    [foo]: http://example.com/  (Optional Title Here)
    
**请注意**：有一个已知的问题是 Markdown.pl 1.0.1 会忽略单引号包起来的链接 title。

链接网址也可以用尖括号包起来：

    [id]: <http://example.com/>  "Optional Title Here"
    
你也可以把 title 属性放到下一行，也可以加一些缩进，若网址太长的话，这样会比较好看：

    [id]: http://example.com/longish/path/to/resource/here
        "Optional Title Here"
 
 
网址定义只有在产生链接的时候用到，并不会直接出现在文件之中。

链接辨别标签可以有字母、数字、空白和标点符号，但是并不区分大小写，因此下面两个链接是一样的：

    [link text][a]
    [link text][A]
    
隐式链接标记功能让你可以省略指定链接标记，这种情形下，链接标记会视为等同于链接文字，要用隐式链接标记只要在链接文字后面加上一个空的方括号，如果你要让 "Google" 链接到 google.com，你可以简化成：

    [Google][]  

然后定义链接内容：

    [Google]: http://google.com/

由于链接文字可能包含空白，所以这种简化型的标记内也许包含多个单词：

    Visit [Daring Fireball][] for more information.

然后接着定义链接：

    [Daring Fireball]: http://daringfireball.net/
    
链接的定义可以放在文件中的任何一个地方，我比较偏好直接放在链接出现段落的后面，你也可以把它放在文件最后面，就像是注解一样。

 参考式链接的好处： 
> 使用 Markdown的参考式链接，可以让文件更像是浏览器最后产生的结果，让你可以把一些标记相关的元数据移到段落文字之外，你就可以增加链接而不让文章的阅读感觉被打断。


----------

### 代码


如果要标记一小段行内代码，你可以用反引号把它包起来（`），例如：
    
    Use the `printf()` function.
会产生：
Use the `printf()` function.

    
如果要在代码区段内插入反引号，你可以用多个反引号来开启和结束代码区段：

    ``There is a literal backtick (`) here.``

这段语法会产生：

``There is a literal backtick (`) here.``

    
代码区段的起始和结束端都可以放入一个空白，起始端后面一个，结束端前面一个，这样你就可以在区段的一开始就插入反引号：

    A single backtick in a code span: `` ` ``
    
    A backtick-delimited string in a code span: `` `foo` ``


会产生：

A single backtick in a code span: `` ` ``

A backtick-delimited string in a code span: `` `foo` ``


    
在代码区段内，& 和尖括号都会被自动地转成 HTML 实体，这使得插入 HTML 原始码变得很容易，Markdown 会把下面这段：

Please don't use any `<blink>` tags.

    <p>Please don't use any <code>&lt;blink&gt;</code> tags.</p>
    
你也可以这样写：

    `&#8212;` is the decimal-encoded equivalent of `&mdash;`.

以产生：
<p><code>&amp;#8212;</code> is the decimal-encoded
equivalent of <code>&amp;mdash;</code>.</p>


---------
<span id = "3.11">
### 图片
</span>

Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： 行内式和参考式。
行内式的图片语法看起来像是：

    ![Alt text](/path/to/img.jpg)
    
    ![Alt text](/path/to/img.jpg "Optional title")

详细叙述如下：

 - 一个惊叹号 !
 - 接着一个方括号，里面放上图片的替代文字
 - 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上 选择性的 'title' 文字。
 
参考式的图片语法是这样的：

    ！[Alt text][id]

「id」是图片参考的名称，图片参考的定义方式则和连结参考一样：

    [id]: url/to/image  "Optional title attribute"

到目前为止， Markdown 还没有办法指定图片的宽高，如果你需要的话，你可以使用普通的 <img> 标签。

-----------
<span id = "3.12">
### 其他
</span>

<span id = "3.12.1">
#### 自动链接
</span>

Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用尖括号包起来：

    <http://example.com/>
    
-------------
<span id = "3.12.2">
#### 反斜杠
</span>

Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用` <em>` 标签），你可以在星号的前面加上反斜杠：

    \*literal asterisks\*

Markdown产出为：
    \*literal asterisks\*
    
Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    #   井字号
    +   加号
    -   减号
    .   英文句点
    !   惊叹号