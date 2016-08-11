# 我的前端学习笔记

记录小白学习前端的过程，如有错误，万望指正，感激不尽。

## 2016.8.11

### 文档流与定位

HTML 中所有元素从左至右，从上往下布局排列，行内元素不换行，块级元素独占一行，这叫做文档流。

CSS 有三种定位机制:普通流，浮动定位和绝对定位。普通流定位中，垂直的块级元素之间的距离由块级元素的垂直外边距确定(注意[外边距合并](http://www.w3school.com.cn/css/css_margin_collapsing.asp)，只有文档流中的框才会发生外边距合并，其他布局类型都不会。

浮动元素、绝对定位和固定定位会使元素脱离文档流，文档流中的其他元素将忽略它，并占用它原本的空间。绝对定位表示相对于它的包含块(理解为父视图)定位，固定定位表示相当于浏览器窗口本身定位。

浮动框表示这个框向某个方向浮动，直到遇到边框位置，利用 `float` 属性控制浮动方向。浮动框所在的行会缩短，形成“文本围绕图像”的效果，如果想避免，可以使用 `clear` 属性，它的取值可以是左右，两边、均无或继承父元素，表示该方向上不允许出现浮动元素。比如，如果声明了左侧清除，表示该元素上外边框的边界，恰好在左侧浮动元素的下外边距的边界之下。

[参考资料](http://www.w3school.com.cn/css/css_positioning.asp)

## 2016.8.10

### CSS 布局盒模型

![](http://www.w3school.com.cn/i/ct_boxmodel.gif)

元素框的整体大小由元素长度、宽度，加上内边距和外边距构成。内外边距可以指定为四个边具有共同的值，也可以单独赋值。

利用 `border-style` 可以设置四个边框的样式，也可以单独设置某一条边。一下两种写法等价:

```css
p {border-style: solid solid solid none;}
p {border-style: solid; border-left-style: none;}
```

## 2016.8.9

### Nodejs 模块管理

Nodejs 的模块可以分为两大类，原生模块和文件模块。原生模块是指 Nodejs 内置的模块，以二进制格式存储，加载速度快。文件模块为用户自己编写，结尾是 `.js` 或 `.node` 或 `.json` 格式的模块，利用 `fs` 模块加载，速度略慢。这两种模块都有缓存，所以重复加载不损失性能。

当我们启动 nodejs 时，主模块的名字作为参数传递到 `_load` 方法中，所以不需要 require 主模块的名字。

如果开发前后端一致的模块，需要注意将类库模块封装到一个闭包中，避免污染全局变量。

## 2016.8.8

### Span 标签

span 标签用来为行内元素提供格式，它单独使用没有任何效果，只有配合 `id` 或 `class` 属性才能产生效果。比如:

```html
<p>My name is <span class="name">bestswifter</span>.</p>
```

### CSS 选择器

利用 CSS 文件可以对 HTML 代码进行精确的样式控制。最基础的两种写法如下:

```css
selector descendant_selector  {
    font-size: 32px;
}
```

选择器有三种写法:

1. `tag_name`: 比如 `p`，`em` 等，用来为指定的标签提供格式
2. `#id_value`，比如 `#bestswifter`，用来为所有 `id` 值为 `bestswifter` 的标签提供格式。
3. `.class_name`，比如 `.coder`，用来为所有 `class` 值为 `coder` 的标签提供格式。

选择器后面可以接派生选择器，两者可以组合成上下文环境，比如:

```html
<div id="welcome" class="step" data-x="-2000" data-y="-2500">
    <p><span class="title"><b>Kt 的开发小站</b></span></p>
</div>
```

对应的 CSS 代码就是: 

```css
#welcome .title {
    font-size: 80px;
    line-height: 150px;
}
```

选择器后面还可以加上中括号，这叫做属性选择器，用来对标签的属性进行过滤，比如:

```css
a[target] { } /* 用来对所有具有属性 target 的 a 标签提供格式*/
a[target="_blank"] { } /* 用来对所有具有属性 target 且值为 _blank 的 a 标签提供格式*/
```

中括号可以不依赖于选择器单独存在，表示一种通配，其中的内容也比较多，详见 [CSS 属性选择器详解](http://www.w3school.com.cn/css/css_selector_attribute.asp)。

### 用命令行进行搜索

`grep` 用来搜索文件中的指定内容。基本语法如下:

```bash
grep '<content>'  <serach_path>
# 例如: grep 'hello' ~/Desktop/article.txt
```

`grep` 命令有几个参数，作用如下:

`-r` 表示递归搜索，使用这个参数时，路径不一定是文件，也可以是一个文件夹。
`-c` 显示单词出现次数而不是出现的上下文内容。
`-w` 只搜索单词，避免待搜索内容出现在某个单词中的情况。
`-l` 显示出现的位置(行数)
`--color` 用彩色显示搜索内容

与 `grep` 类似的命令还有 `ggrep`，它是 `git grep` 的别名，和 `grep` 的区别在于，它只会搜索 git 索引的文件，而且可以指定在某个版本中搜索。

如果要查询 commit 记录中的关键字，可以使用 `git log` 命令:

1. 两天前的提交历史：git log --since=2.days
2. 指定作者为"BeginMan"的所有提交:$ git log --author=BeginMan
3. 指定关键字为“init”的所有提交：$ git log --grep=init
4. 如指定提交者为"Jack"的所有提交：$ git log --committer=Jack

参考文章: [HowTo: Use grep Command](http://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)、[git log常用命令以及技巧](http://www.cnblogs.com/BeginMan/p/3577553.html)。

### npm 安装

运行 `npm install` 进行本地安装之前，首先需要执行 `npm init`，它会引导我们生成一个 `package.json` 文件，这个文件中可以指定我们工程的 git 地址，项目依赖等信息。所以在发布的过程中，并不需要提交 `node_modules` 文件夹中的内容，别人下载下来后运行 `npm init` 即可。

为了实现这一点，我们在安装第三方 package 时需要加上 `--save` 参数，表示把依赖关系写入到 `package.json` 文件中去。

## 2016.8.7

### Nodejs 入门

阅读完了 《Nodejs 入门》这本书，非常适合新手，主要讲解了路由、模块，非阻塞调用，Nodejs 服务端等基础内容。

npm 是一个强大的包管理机制，所谓的包，也叫 package 或者 module，其实就是别人写的几个文件的集合，对外提供了一些服务。

通过 `npm install` 命令可以安装别人的包，分为本地安装和全局安装两种。如果是 `require("xxx")` 这种写法则使用本地安装，如果是安装 grunt 这样的命令行工具则使用全局安装。区别是安装位置不同。

本地安装后会在当前目录生成 `node_modules` 文件夹，`package.json` 文件用来描述包的配置，类似于 Cocoapods 中的 Podfile 文件，可以指定安装版本号等信息。

### Nodejs 设计思想

Nodejs 是一个单线程的执行环境，依赖于事件异步 I/O 机制，这要求我们在编程的过程中需要主义思想的转变。

我们首先可以创建路由模块，根据不同的访问路径调用不同的处理模块。但是在路由分发时必须注意使用异步回调，不能让子模块返回处理结果给路由模块，否则会造成线程阻塞。正确的做法是把 `request` 和 `response` 对象发给对应的处理模块，然后由他们去处理。

### 表单提交图片

```html
<form action="demo_form.asp">
  Select Image: <input type="file" name="upload_image"><br>
  Submit: <input type="submit" value="Submit">
</form>
```

首先放在表单标签中，`action` 属性表示提交地址，还可以指定以 GET 还是 POST 提交。

第一行表示要提交一个文件，这里千万要设置 `name` 属性，只有设置了这个属性，内容才会被提交。Server 端依赖于这个属性获取信息。

第二行表示提交按钮。

在 Nodejs 中可以使用 `formidable` 这个库：

```js
var form = new formidable.IncomingForm();
form.parse(request, function (error, fields, files)  {
  console.log(files.upload_image.path);
  fs.renameSync(files.upload_image.path, "/tmp/test.png"); // 重命名
});
```

这里 `files` 表示提交的文件，其中的 `upload_image` 就是 HTML 当时设置的 name 属性。两者需要对应

### NodeJs 初印象

利用 V8 引擎提供了 JavaScript 的运行环境，用于后台开发。用法上类似于 PHP、Python 实现动态网页。

NodeJs 自带了很多模块，比如 HTTP、URL 等模块。也可以自己实现一个模块，只要写上：

```js
exports.内部方法名 = 外部方法名
```

然后外部使用：

```js
var 模块名= require("模块路径")
模块名.方法名()
```

## 2016.8.6

### JavaScript 对象

在JavaScript中，对象就是一个键/ 值对的集合 -- 你可以把JavaScript的对象想象成一个键为字符串类型的字典。JS 对象的方法可以做为某个键值对的值。

### 让 a 标签点击后执行一段代码

```html
<a href="javascript:void(0);" onclick="alert(1);">点击一</a> 
```

首先加上 `href` 属性让文字看上去可点击，点击后跳转到 js 代码执行结果所对应的网页，比如 `javascript:myFunction();`。这里的 `void(0` 
表示一个 undefined 值。所以不会有任何跳转，于是就可以执行 `onclick` 属性中的 js 代码了。

或者也可以写为： 

```html
<a id="myLink" href="#" onclick="MyFunction();return false;">link text</a>
```

`return false` 表示会阻止浏览器跳转到 `href` 属性所表示的地址。

最好的方案当然是使用 jQuery。

参考地址：[JavaScript function in href vs. onclick](http://stackoverflow.com/questions/1070760/javascript-function-in-href-vs-onclick)
