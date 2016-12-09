# Android 学习日常总结

## 2016-12-09

### AS 快捷键（2）

* 查看被调用的地方：`option + F7`

### validate() 与 requestLayout()

![](http://images.bestswifter.com/1734948-b4493f7b0234dd69.jpg)

如果View确定自身不再适合当前区域，比如说它的LayoutParams发生了改变，需要父布局对其进行重新测量、布局、绘制这三个流程，往往使用requestLayout。而invalidate则是刷新当前View，使当前View进行重绘，不会进行测量、布局流程，因此如果View只需要重绘而不需要测量，布局的时候，使用invalidate方法往往比requestLayout方法更高效。

## 2016-11-30

### AS 快捷键

* 查看函数调用栈：`Control + Option + H`
* 查看类的方法和属性：`Command + F12`
* 页面浏览前进、后退：`Command + [/]`
* (Vim) `Ctrl-]` 查看定义,`Ctrl-o` 返回
