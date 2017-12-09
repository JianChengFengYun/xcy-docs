## 认识 Markdown

> Markdown 是一种轻量级的「标记语言」，它的优点很多，目前也被越来越多的写作爱好者，撰稿者广泛使用。看到这里请不要被「标记」、「语言」所迷惑，Markdown 的语法十分简单。常用的标记符号也不超过十个，这种相对于更为复杂的 HTML 标记语言来说，Markdown 可谓是十分轻量的，学习成本也不需要太多，且一旦熟悉这种语法规则，会有一劳永逸的效果。



### 使用 Markdown 的优点

- 专注你的文字内容而不是排版样式，安心写作。
- 轻松的导出 HTML、PDF 和本身的 .md 文件。
- 纯文本内容，兼容所有的文本编辑器与字处理软件。
- 随时修改你的文章版本，不必像字处理软件生成若干文件版本导致混乱。
- 可读、直观、学习成本低。



### 一、区块元素

#### 1. 段落
前后要有一个以上的空行,普通段落不该用空格或制表符来缩进。

#### 2. 标题

Markdown 支持两种标题的语法，类 Setext 和类 atx 形式


- **类 Setext 形式**

    是用底线的形式，利用 = （最高阶标题）和 - （第二阶标题），例如：

    ```
    This is an H1 
    =============
    
    This is an H1
    =
    
    This is an H2
    --------------
    ```


任何数量的 = 和 - 都可以有效果。


- **类 Atx 形式**

    是在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶，例如：

    ```
    # 这是 H1
    
    ## 这是 H2
    
    ###### 这是 H6
    ```

#### 3. 区块引用 

- **在每行的最前面加上 >** ：


```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.
```

- **Markdown 也允许只在整个段落的第一行最前面加上 >**：

```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
id sem consectetuer libero luctus adipiscing.
```

区块引用可以嵌套：


```
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
```

引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等：


```
> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");
```


#### 4. 列表

Markdown 支持有序列表和无序列表。

- **无序列表**

    使用星号( * )、加号( + )或是减号( - )作为列表标记：


```
*   Red
*   Green
*   Blue

等同于：

+   Red
+   Green
+   Blue

也等同于：

-   Red
-   Green
-   Blue
```

- **有序列表**
    
    数字 + 一个英文句点 + 空格：


```
1.  Bird
2.  McHale
3.  Parish
```

*****列表标记上使用的数字并不会影响输出的 HTML 结果**

如果你的列表标记写成：


```
1.  Bird
1.  McHale
1.  Parish
```

或甚至是：


```
3. Bird
1. McHale
8. Parish
```
都会得到完全相同的 HTML 输出

*****建议第一个项目最好还是从 1. 开始，因为 Markdown 未来可能会支持有序列表的 start 属性。**

列表项目标记通常是放在最左边，但是其实也可以缩进，最多 3 个空格，项目标记后面则一定要接着至少一个空格或制表符。


``` 
要让列表看起来更漂亮，你可以把内容用固定的缩进整理好：

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.

但是如果你懒，那也行：

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
Suspendisse id sem consectetuer libero luctus adipiscing.
```







列表项目可以包含多个段落，每个项目下的段落都必须缩进 4 个空格或是 1 个制表符：


```
1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.
```



如果你每行都有缩进，看起来会看好很多，当然，Markdown 也允许仅仅段落第一行缩进：


```
*   This is a list item with two paragraphs.

    This is the second paragraph in the list item. You're
only required to indent the first line. Lorem ipsum dolor
sit amet, consectetuer adipiscing elit.

*   Another item in the same list.
```

如果要在列表项目内放进引用，那 > 就需要缩进：


```
*   A list item with a blockquote:

    > This is a blockquote
    > inside a list item.
```



如果要放代码区块的话，该区块就需要缩进两次，也就是 8 个空格或是 2 个制表符：


```
*   一列表项包含一个列表区块：

        <代码写在这>
```

当然，项目列表很可能会不小心产生，像是下面这样的写法：


```
1986. What a great season.
```

换句话说，也就是在行首出现数字-句点-空白，要避免这样的状况，你可以在句点前面加上反斜杠。


    1986\. What a great season.


#### 5. 代码框

- **行内代码**(inline code)：\`\<code>`

    inline `<code>`

- **块代码**(block code)的写法：简单地缩进 4 个空格或是 1 个制表符

- **Fenced Code Block** 写法是：第一行和最后一行都是3个 " ` "，中间的行是代码，

    加上语言类型，代码块有高亮显示


    ``` js
    <code>
    ```






#### 6. 分隔线

在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：


```
* * *

***

*****

- - -

---------------------------------------
```

#### 7. 表格

表格是我觉得 Markdown 比较累人的地方，例子如下：

```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

```
这种语法生成的表格如下：

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |



两边有冒号是居中，Cool 右边有冒号是靠右对齐

### 二、区段元素

#### 1. 链接

Markdown 支持两种形式的链接语法： 行内式和参考式(链接标记、隐士链接标记)。<br />不管是哪一种，链接文字都是用 [方括号] 来标记。

- **行内式**

\[链接文字](插入网址链接 "Title")

链接的 title 文字可以不加：

```
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
```

如果链接到同样主机的资源，你可以使用相对路径：

    See my [About](/about/) page for details.

- **参考式**

\[链接文字][链接的标记]

    This is [an example][id] reference-style link.

可以选择性地在两个方括号中间加上一个空格：

    This is [an example] [id] reference-style link.

接着，在文件的任意处，你可以把这个标记的链接内容定义出来：

    [id]: http://example.com/  "Optional Title Here"

**链接内容定义的形式为：**

> 1. 方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接文字
> 2. 接着一个冒号<br />
> 3. 接着一个以上的空格或制表符<br />
> 4. 接着链接的网址
> 5. 选择性地接着 title 内容，可以用单引号、双引号或是括弧包着

下面这三种链接的定义都是相同：

    [foo]: http://example.com/  "Optional Title Here"
    [foo]: http://example.com/  'Optional Title Here'
    [foo]: http://example.com/  (Optional Title Here)

*****请注意：有一个已知的问题是 Markdown.pl 1.0.1 会忽略单引号包起来的链接 title。**

链接网址也可以用方括号包起来：

    [id]: <http://example.com/>  "Optional Title Here"

你也可以把 title 属性放到下一行，也可以加一些缩进，若网址太长的话，这样会比较好看：

    [id]: http://example.com/longish/path/to/resource/here
        "Optional Title Here"

网址定义不会直接出现在文件之中。

链接标记可以有字母、数字、空白和标点符号，但是并不区分大小写，因此下面两个链接是一样的：

    [link text][a]
    [link text][A]

**隐式链接标记**

可以省略指定链接标记，这种情形下，链接标记等同于链接文字，在链接文字后面加上一个空的方括号，如果要让 "Google" 链接到 google.com，你可以简化成：

```
[Google][]

然后定义链接内容：

[Google]: http://google.com/
    
```

由于链接文字可能包含空白，所以这种简化型的标记内也许包含多个单词：

```
Visit [Daring Fireball][] for more information.

然后接着定义链接：

[Daring Fireball]: http://daringfireball.net/
```

链接的定义可以放在文件中的任何一个地方,直接放在链接出现段落的后面，你也可以把它放在文件最后面，就像是注解一样。

**下面是三种写法的例子，提供作为比较之用：**

- 参考式链接的范例：

    ```
    I get 10 times more traffic from [Google] [1] than from
    [Yahoo] [2] or [MSN] [3].
      [1]: http://google.com/        "Google"
      [2]: http://search.yahoo.com/  "Yahoo Search"
      [3]: http://search.msn.com/    "MSN Search"
    ```

- 如果改成用链接名称的方式写：

    ```
    I get 10 times more traffic from [Google][] than from
    [Yahoo][] or [MSN][].
      [google]: http://google.com/        "Google"
      [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
      [msn]:    http://search.msn.com/    "MSN Search"
    ```

- 下面是用行内式写

    ```
    I get 10 times more traffic from [Google](http://google.com/ "Google")
    than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or
    [MSN](http://search.msn.com/ "MSN Search").
    ```

#### 2. 强调

Markdown 使用星号（*）和底线（_）作为标记强调字词的符号，被 * 或 _ 包围的字词会被转成用 \<em> 标签包围，用两个 * 或 _ 包起来的话，则会被转成 \<strong>，例如：


```
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__
```
会转成：


```
<em>single asterisks</em>

<em>single underscores</em>

<strong>double asterisks</strong>

<strong>double underscores</strong>

```

强调也可以直接插在文字中间：

    un*frigging*believable

但是如果你的 * 和 _ 两边都有空白的话，它们就只会被当成普通的符号。

如果要在文字前后直接插入普通的星号或底线，你可以用反斜线：

    \*this text is surrounded by literal asterisks\*




#### 3. 图片

Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： 行内式和参考式。

- **行内式**


```
![Alt text](/path/to/img.jpg)

![Alt text](/path/to/img.jpg "Optional title")

``` 
>详细叙述如下：
> 1. 一个惊叹号 !
> 2. 接着一个方括号，里面放上图片的替代文字
> 2. 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上 选择性的 'title' 文字。

- **参考式**的图片语法则长得像这样：


    ![Alt text][id]

「id」是图片参考的名称，图片参考的定义方式则和连结参考一样：

    [id]: url/to/image  "Optional title attribute"

到目前为止， Markdown 还没有办法指定图片的宽高，如果你需要的话，你可以使用普通的 \<img> 标签。


#### 4. 其它


- **反斜杠**

Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用 \<em> 标签），你可以在星号的前面加上反斜杠：

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



### 三、兼容 HTML



不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写。

只有一些 HTML 区块元素――比如 \<div>、\<table>、\<pre>、\<p> 等标签，必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能缩进。

Markdown 的生成器有足够智能，不会在 HTML 区块标签外加上不必要的 \<p> 标签。

例子如下，在 Markdown 文件里加上一段 HTML 表格：


```
这是一个普通段落。

<table>
    <tr>
        <td>Foo</td>
    </tr>
</table>

这是另一个普通段落。

```


在 HTML 区块标签间的 Markdown 格式语法将不会被处理。比如，你在 HTML 区块内使用 Markdown 样式的 * 强调 * 会没有效果。

HTML 的区段（行内）标签如 \<span>、\<cite>、\<del> 可以在 Markdown 的段落、列表或是标题里随意使用。

依照个人习惯，甚至可以不用 Markdown 格式，而直接采用 HTML 标签来格式化。举例说明：如果比较喜欢 HTML 的 \<a> 或 \<img> 标签，可以直接使用这些标签，而不用 Markdown 提供的链接或是图像标签语法。

和处在 HTML 区块标签间不同，Markdown 语法在 HTML 区段标签间是有效的。

例如：

```html
<html>
<!--在这里插入内容-->
<a href="http://www.baidu.com">百度一下</a>
</html>

```

```html

<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=#00ffff size=3>null</font>
<font color=gray size=5>gray</font>

```

线上链接：
[我的MarkDown学习之旅](https://segmentfault.com/a/1190000011591534)

例如：怎样用MarkDown写一个npmjs里面的README.md文档？？？

README 应该是介绍code source 的一个概览.其实这个静态文件是有约定成俗的规范：

- 环境依赖

- 项目介绍

- 代码实现了什么功能?

- 特性

- 如何使用? (系统环境参数,部署要素)

- 代码组织架构是什么样的?

- 版本更新重要摘要

```
DEMO
===========================

###########环境依赖
node v0.10.28+
reids ~

###########部署步骤
1. 添加系统环境变量
    export $PORTAL_VERSION="production" // production, test, dev


2. npm install  //安装node运行环境

3. gulp build   //前端编译

4. 启动两个配置(已forever为例)
    eg: forever start app-service.js
        forever start logger-service.js


###########目录结构描述
├── Readme.md                   // help
├── app                         // 应用
├── config                      // 配置
│   ├── default.json
│   ├── dev.json                // 开发环境
│   ├── experiment.json         // 实验
│   ├── index.js                // 配置控制
│   ├── local.json              // 本地
│   ├── production.json         // 生产环境
│   └── test.json               // 测试环境
├── data
├── doc                         // 文档
├── environment
├── gulpfile.js
├── locales
├── logger-service.js           // 启动日志配置
├── node_modules
├── package.json
├── app-service.js              // 启动应用配置
├── static                      // web静态资源加载
│   └── initjson
│   	└── config.js 		// 提供给前端的配置
├── test
├── test-service.js
└── tools



###########V1.0.0 版本内容更新
1. 新功能	 aaaaaaaaa
2. 新功能	 bbbbbbbbb
3. 新功能	 ccccccccc
4. 新功能	 ddddddddd

```