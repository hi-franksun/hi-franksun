# HTML overview

HTML (HyperText Markup Language) 不是一门编程语言，而是一种用来告知浏览器如何组织页面的标记语言。HTML 可复杂、可简单，一切取决于开发者。它由一系列的元素（elements）组成，这些元素可以用来包围不同部分的内容，使其以某种方式呈现或者工作。 一对标签（ tags）可以为一段文字或者一张图片添加超链接，将文字设置为斜体，改变字号，等等。

> `<!DOCTYPE html>`: 声明文档类型
> HTML 标签不区分大小写。也就是说，输入标签时既可以使用大写字母也可以使用小写字母。例如，标签 `<title>` 写作 `<title>`、`<TITLE>`、`<Title>`、`<TiTlE>`，等等都可以正常工作。不过，从一致性、可读性等各方面来说，最好仅使用小写字母。
> HTML注释 `<!-- 注释内容 -->`
> 使用单引号和双引号都可以，但是为了方便，推荐使用双引号，如果要显示一些特殊字符，需要使用特殊的字符；

一个 `元素(element)` 包括：
| 开始标签(Opening tag) | 内容(Content) | 结束标签(Closing tag) |
| -- | -- | -- |
| `<h1>` | `一级标题` | `</h1>` |

1. 开始标签（Opening tag）：包含元素的名称（本例为 p），被左、右角括号所包围。表示元素从这里开始或者开始起作用 —— 在本例中即段落由此开始。
2. 结束标签（Closing tag）：与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾 —— 在本例中即段落在此结束。初学者常常会犯忘记包含结束标签的错误，这可能会产生一些奇怪的结果。
3. 内容（Content）：元素的内容，本例中就是所输入的文本本身。
4. 元素（Element）：开始标签、结束标签与内容相结合，便是一个完整的元素。

## 基本使用

1. 嵌套元素
    元素之间可以嵌套，只要保证 tag 开闭顺序即可：`<div><h1><strong>this is h1</strong></h1></div>`
块级元素和内联元素

2. 块级元素和内联元素
    - 块级元素：一般会换行，比如 `<h1>,<lu>,<li>`
    - 内连元素：一般不会主动换行，例如超链接元素 `<a>` 或者强调元素 `<em>` 和 `<strong>`。

3. 空元素
    有一些 tag 是没有关闭元素的，通常是链接、图片这类信息
    `<img src="https://roy-tian.github.io/learning-area/extras/getting-started-web/beginner-html-site/images/firefox-icon.png">`

4. 属性
    `<div class="class name" name="edite note" id="test" >this is div block</div>`
    属性包含元素的额外信息，这些信息不会出现在实际的内容中。在上述例子中，这个class属性给元素赋了一个识别的名字（id），这个名字此后可以被用来识别此元素的样式信息和其他信息。

    一个属性必须包含如下内容：

    1. 一个空格，在属性和元素名称之间。(如果已经有一个或多个属性，就与前一个属性之间有一个空格。)
    2. 属性名称，后面跟着一个等于号。
    3. 一个属性值，由一对引号“ ”引起来。

5. 布尔属性
    很少使用，知道就行，比如下边这个就会显示一个灰色的输入框，不能输入任何内容；
    `<input type="text" disabled="disabled">`

## 头元素

什么是 HTML 头部元素?

### 标题

`<title>title name </title>`

### `<meta>` 元素

- charset
    `<meta charset="utf-8">`
- name
- content
    `<meta name="author" content="Chris Mills">`
    `<meta name="description" content="The MDN Learning Area aims to provide complete beginners to the Web with all they need to know to get started with developing web sites and applications.">`
    > description 信息有助于搜索引擎进行抓取，seo 优化很有帮助
- icon
    `<link rel="shortcut icon" href="favicon.ico" type="image/x-icon">`
- CSS
    `<link rel="stylesheet" href="my-css-file.css">`
- JavaScrip
    `<script src="my-js-file.js"></script>`
- langage，在 html 标签添加
    `<html lang="zh-CN">`

## 文字处理

为什么需要语义标签，因为语义标签更加符合显示世界，也是为了更好的阅读，使用语义标签，特别是搜索引擎和残障人士阅读的时候，这个一个规范，但是现在这种规范使用的越来越少，因为都是组件化开发，虽然 `div` 能些所有的东西，但是能语义化还是尽量使用语义化，这个是标准，有没有追求很重要；

标题

`<h1>、<h2>、<h3>、<h4>、<h5>、<h6>`。每个元素代表文档中不同级别的内容; `<h1>` 表示主标题（the main heading），`<h2>` 表示二级子标题（subheadings），`<h3>` 表示三级子标题（sub-subheadings），等等。

段落
`<p>` 表示段落的标签

列表
列表list：`<li></li>` >li默认是无序的，一般包含在 ul 和 ol 标签内使用
无序unordered：`<ul></ul>`
有序ordered：`<ol></ol>`
嵌套列表：就是 ul 和 ol 互相嵌套，li 标签是不变化的；

强调

- `</strong> <em>`，这些也就是内联标签，主要用来对文本进行标记，也可以说不会换行的。
- `<i>` 被用来传达传统上用斜体表达的意义：外国文字，分类名称，技术术语，一种思想……
- `<b>` 被用来传达传统上用粗体表达的意义：关键字，产品名称，引导句……
- `<u>` 被用来传达传统上用下划线表达的意义：专有名词，拼写错误……

引用
> 注意：URL可以指向HTML文件、文本文件、图像、文本文档、视频和音频文件以及可以在网络上保存的任何其他内容。 如果浏览器不知道如何显示或处理文件，它会询问您是否要打开文件（需要选择合适的本地应用来打开或处理文件）或下载文件（以后处理它）。

<a href="https://www.mozilla.org/zh-CN/">Mozilla 主页</a>
的超链接

统一资源定位符(URL)与路径(path)快速入门
要全面地了解链接目标，你需要了解统一资源定位符和文件路径。本小节将介绍相关的信息。

统一资源定位符（英文：Uniform Resource Locator，简写：URL）是一个定义了在网络上的位置的一个文本字符串。例如 Mozilla 的中文主页定位在 `https://www.mozilla.org/zh-CN/.`

URL使用路径查找文件。路径指定文件系统中您感兴趣的文件所在的位置。看一下一个简单的目录结构的例子（源码可查看 创建超链接（creating-hyperlinks）。

[MDN 最佳实践](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks)

> 其实html 文本标记语言和 markdwon 标记语言的本质是一样的，你是熟练使用 markdown 语言，且有追求，那么基本上 html 标记语言，就不会出很大问题，特别是语义化的时候；

### 其他更多标记可以看文档，没必要全部记下来，而且现在使用的过程中，很多都是用组件化开发，html 的语义被弱化了；

