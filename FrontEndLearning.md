# 我的前端学习笔记

记录小白学习前端的过程，如有错误，万望指正，感激不尽。

## 2016-11-28

### HTTP 协议的几个字段
1. keep-alive：这个字段在 http 1.0 时代存在，可以复用 TCP 链接。HTTP 1.1 默认都是 keep-alive，不过由于历史原因，有时习惯性保留
2. 对于持久连接，要用 content-length 告诉服务器数据是否传输结束，如果 length 太小会导致内容被截断，如果过大会导致 pending
3. content-length 不适用于动态生成的内容，为了解决这个问题，我们使用 `Transfer-Encoding: chunked`, 表示把数据分块，每块都用 16 进制标记了内容的长度，如果为 0 说明到了末尾。

比如：

 ```js
sock.write('HTTP/1.1 200 OK\r\n');
sock.write('Transfer-Encoding: chunked\r\n');
sock.write('\r\n');

sock.write('b\r\n');
sock.write('01234567890\r\n');

sock.write('5\r\n');
sock.write('12345\r\n');

sock.write('0\r\n');
sock.write('\r\n');
```

## 2016.9.5

### 长连接、keep-alive、多路复用

长连接表示打开一个 TCP 连接，避免每次 HTTP 请求时的 TCP 握手。长连接通常通过心跳包来维持。keep-alive 是一种长连接的实现，不过不是用心跳包，而是在一定时间后关闭。这个超时时间通常由 Web 服务器指定，比如 Apache 默认是 15s，过长的超时时间会增加服务器的负担。

多路复用的基于长连接的思想之上。在传统的长连接实现中，后续请求必须等待前面的请求执行完才被处理，因此很简单的请求可能因为前面的复杂请求而阻塞([Head-of-line blocking problem](https://en.wikipedia.org/wiki/Head-of-line_blocking))。

多路复用的实现是同时处理多个请求，并且将返回结果切分成若干段，用 id 加以标记，返回到客户端后再拼装起来。

## 2016.9.1

### Grunt vs Gulp

Gulp 和 Grunt 是两个用于构建的脚手架工程，前者更新，更快，基于 Node.js 的流机制， 进行内存中的操作。Grunt 更像是在语法和代码层面定义任务，由于要对文件进行 I/O 操作，速度略慢，但是胜在社区发展较好。

在实际选择的时候，可以根据自己喜好进行选择，如果没有特殊原因不需要更换。

### Webpack vs RequireJS

[RequireJS](http://requirejs.org/) 是一个实现了 **AMD**([Asynchronous Module Definition](https://github.com/amdjs/amdjs-api) 规范的 JS 框架，用来解决浏览器端的模块依赖问题，缺点在于代码的书写与阅读比较困难，是一种妥协的实现。

模块加载需要避免两种情况，不能每个文件单独请求，这样造成请求次数过多，也不宜所有文件一次请求，这样容易造成流量浪费。因此最合理的方式是按需动态加载，因此需要对所有模块进行静态分析，编译打包。实际上模块不只局限于 JavaScript 文件，CSS、图片、字体等都是模块，我们可以希望能使用 `require` 的方式进行加载:

```js
require("./style.css");
require("./style.less");
require("./template.jade");
require("./image.png");
```

实际上 webpack 只能处理 JavaScript 模块，其他形式的模块需要通过 loader 转换器编程 webpack 可以处理的模块。

安装 `webpack-dev-server` 这个模块后可以做到自动编译与自动刷新，只要运行:

```c
$ webpack-dev-server --inline --content-base ./
```

此后，保存对文件的时候会触发重编译。由于在本地启动了 node 服务器，因此还可以实现浏览器自动刷新功能。

## 2016.8.29

### HTML canvas

这个元素类似于一块画布，由 JavaScript 在运行时动态绘制图形，比如:

```html
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;"></canvas>
```

```js
var c = document.getElementById("myCanvas");
var ctx = c.getContext("2d");
ctx.fillStyle = "#FF0000";
ctx.fillRect(0,0,150,75);
```

这里的 `2d` 参数表示绘制二维图形。具体使用方法类似于 Core Graphics

## 2016.8.17

### JSX 与 React

React 是一个 Model 层的 JS 框架，利用 Virtual Dom 实现了响应式的编程。有了 React，我们就可以用 JS 来写 UI 代码了。

用一句话来概括就是:“简单的 HTML 代码 + React 框架 + 基于 React 的 JS 代码 = 完整的网页”，比如下面这个例子:

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="build/react.js"></script>
    <script src="build/react-dom.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```

JSX 是 JS 的拓展，或者说是一种预处理状态。JSX 在 JS 的基础上支持 XML 标签。Babel 是一个 JS 的编译器，通过 Babel 可以把带有 React 标签的 JSX 文件编译成 JS 代码。

比如这段 JSX 代码:

```js
var myDivElement = <div className="foo" />;
ReactDOM.render(myDivElement, document.getElementById('example'));
```
编译后的结果是:

```js
var MyComponent = React.createClass({/*...*/});
var myElement = <MyComponent someProperty={true} />;
ReactDOM.render(myElement, document.getElementById('example'));
```

由于 React 本质上还是一个 View 层的框架，所以在 Ejs、Jade  这样的 HTML 模板中也可以正常使用。实际上，React 也是一种模板，它完全可以独立于 Ejs 而单独使用。

## 2016.8.16

### Sails.js 

Sails.js 是一个基于 Express 的 Server 框架，它采用了 MVC 模式。控制器主要负责转发与渲染界面，具体的逻辑由 Service 负责。

View 层使用模板来实现，当 Controller 传来数据时即可渲染成 HTML。

Model 层定义了数据的类型。

[Node.js与Sails~项目结构与Mvc实现](http://www.cnblogs.com/lori/archive/2015/10/13/4874115.html)、[为什么使用Sails?](https://cnodejs.org/topic/553c7b4a1a6e36a27780ee65)

## 2016.8.15

### WebWorker

WebWorker 是一个 HTML5 中在后台执行 JavaScript 代码的技术，这样不会阻塞 UI 界面。使用方法是在 JavaScript 中调用 `postMessage` 方法，然后在 HTML5 中的 JavaScript 中设置 `onmessage` 函数:

```js
var worker = new Worker("worker.js"); //创建一个Worker对象并向它传递将在新线程中执行的脚本的URL
worker.postMessage("hello world");     //向worker发送数据
worker.onmessage = function(evt){     //接收worker传过来的数据函数
    console.log(evt.data);              //输出worker发送来的数据
}
```

此时，`evt` 就是 `postMessage` 方法中的参数。

参考文章: [Web Worker](http://blog.jobbole.com/30592/)

## 2016.8.14

### 构建工具

前端没有固定的集成开发环境(IDE)，实际传输的就是源码。在发布过程中，我们可能会选择取消 CSS 注释和空行，从而提高页面性能。有时候也会把多张图片拼合起来，当某个图标发生改动时，大的图片也需要改变。

除此以外，我们可能会使用 Jade 作为 HTML 模板，CoffeeScript 作为 JavaScript 方言，SASS 作为 CSS 的预处理器等等。在开发阶段我们写的是 Jade、CoffeeScript 和 SASS，而在发布阶段，实际使用的是 HTML、CSS 和 JavaScript。

上述这些转化的过程统称为:“构建”(build)。它的作用是把原始素材(可能是代码，或者图片)转换成最终可以被浏览器直接使用的形式。

构建工具出现的背景是，一个高性能网页的特点，通常不利于开发。比如为了节省网络请求，我们会建议把多个 CSS 文件合并在一起，然而对于开发者来说，这样显然不利于代码的组织与管理。如果用 import 的方式，虽然实现简单，但又会阻塞请求。

因此，构建工具出现的目的就是，在不影响最终代码效果的同时，尽量简化工程师在开发过程中的步骤，从而提高效率。

[Jade 模板简介](http://naltatis.github.io/jade-syntax-docs/)

## 2016.8.12

### 缓存模块设计

我们可以为静态文件设置缓存，为了避免频繁刷新缓存，可以把浏览器中的缓存时间设置为 10 年。同时为了避免缓存冗余，可以在静态文件的请求后面添加 md5 值，这样当文件内容改变时，浏览器就会自动重新发送请求。

在 Nodejs 这边，可以把静态文件加载进内存中，但同时需要注意 nodejs 实例可能不止一个，应该避免多个实例拥有重复的缓存，从而浪费内存。V8 引擎的垃圾回收机制是暂停 JS 线程并扫描需要回收的内存，因此占用大量内存会导致垃圾回收时间增加。

### Nodejs 异步 I/O

多线程单进程模型的缺点在于线程切换开销较大，状态同步问题容易使问题复杂化。单线程多进程模型的缺点在于多个 I/O 不能分不到多个进程中执行。

I/O 操作分为两步，首先需要读取数据放到系统内核的缓冲区，然后再把内存缓冲区中的内容复制到用户进程区。对于非阻塞 I/O 来说，它会判断缓冲区内是否已有数据，如果没有则立刻返回，如果有数据，会以阻塞的方式将数据复制到用户进程中，因为它们需要在时间就绪后自己负责进行读写。

I/O 多路复用技术是指，利用 select， poll 轮询所有的 I/O 事件。所谓的轮询是指一次遍历所有的 I/O 文件描述符，判断是否已经就绪。一旦发现已经就绪的，函数调用就会返回。这些操作实际上是非阻塞的同步 I/O 操作。

多路复用的另一种实现是 epoll，它不需要在函数返回后，通过遍历来找到就绪的描述符，而是直接调用当初注册的回调函数。epoll 的优点在于能够监视大量的描述符(主要取决于系统内存，1GB 大约支持 10W 个)，但如果没有大量的空闲连接(比如长连接)，epoll 的效率并不明显。

显然非阻塞 I/O 并不等价于异步 I/O，因为前者在将数据从缓存区复制到用户进程时还会阻塞，而异步 I/O 则会自动做好这一点，在数据复制到用户进程后才调用回调函数，中途完全不会阻塞线程。
 
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
`-n` 显示出现的位置(行数)
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
