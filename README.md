# 我的前端学习笔记

### 2016.8.9

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
