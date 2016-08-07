# 我的前端学习笔记

### 2016.8.7

#### Nodejs 入门

阅读完了 《Nodejs 入门》这本书，非常适合新手，主要讲解了路由、模块，非阻塞调用，Nodejs 服务端等基础内容。

npm 是一个强大的包管理机制，所谓的包，也叫 package 或者 module，其实就是别人写的几个文件的集合，对外提供了一些服务。

通过 `npm install` 命令可以安装别人的包，分为本地安装和全局安装两种。如果是 `require("xxx")` 这种写法则使用本地安装，如果是安装 grunt 这样的命令行工具则使用全局安装。区别是安装位置不同。

本地安装后会在当前目录生成 `node_modules` 文件夹，`package.json` 文件用来描述包的配置，类似于 Cocoapods 中的 Podfile 文件，可以指定安装版本号等信息。

#### Nodejs 设计思想

Nodejs 是一个单线程的执行环境，依赖于事件异步 I/O 机制，这要求我们在编程的过程中需要主义思想的转变。

我们首先可以创建路由模块，根据不同的访问路径调用不同的处理模块。但是在路由分发时必须注意使用异步回调，不能让子模块返回处理结果给路由模块，否则会造成线程阻塞。正确的做法是把 `request` 和 `response` 对象发给对应的处理模块，然后由他们去处理。

#### 表单提交图片

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

#### NodeJs 初印象

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

### 2016.8.6

#### JavaScript 对象

在JavaScript中，对象就是一个键/ 值对的集合 -- 你可以把JavaScript的对象想象成一个键为字符串类型的字典。JS 对象的方法可以做为某个键值对的值。

#### 让 a 标签点击后执行一段代码

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
