# 我的 JavaScript 学习笔记

## 2017-01-24

### var 和 let

var 的作用域是它所定义的函数，而 let 则是所处的最近一个 block 块，比如： 

```js
for(var i = 1; i < 6; i++) {
      document.getElementById('my-element' + i).addEventListener('click', function() { alert(i) })
}
```

此时的 var i 在 for 循环内部和外部都有效，它的作用域是最近的一个 function（或者 global），因此 alert(i) 中捕获的是同一个变量 i（虽然 i 一直在变），这就导致了最终所有点击事件输出的 i 的值都一样。

解决方案是包一层函数：

```javascript
function createfunc(i) {
    return function() { alert(i) };
}

for(var i = 1; i < 6; i++) {
    document.getElementById('my-element' + i).addEventListener('click', createfunc(i))
}
```

这样的好处是 i 会被 `createfunc` 捕获成局部变量，再被 block 捕获，因此每个 i 的值都会不一样。

如果是在 node.js 或者 babel 环境下写 js，就可以使用 ES6 最新的 let 关键字。let 关键字会保证 i 的作用于仅限于 for 循环（这也是符号我们的一般认识）

这时候所有的

## 2017-01-20

### Atom 更新插件报错 Permission Denied

执行：`sudo chown -R `whoami` ~/.atom`

## 2017-01-19

### undefined 和 null

这两者差距非常小， 主要是由于历史原因导致的。他们在 if 判断中都作为否定判断，如果执行 `undefined == null` 还会得到肯定的比较结果。

在实际使用时，建议用 null 表示不应该有值，用 undefined 表示应该有值，但还没有赋值

### this 详解

JS 中的 this 表示当前函数的上下文，但是 JS 调用函数一共有四种方式：

* function invocation: alert('Hello World!')
* method invocation: console.log('Hello World!')
* constructor invocation: new RegExp('\\d')
* indirect invocation: alert.call(undefined, 'Hello World!')

函数调用可以理解为全局函数，如果是对象的方法，那就是第二种情况：方法调用。在函数调用中，this 表示全局对象。在 Strict mode 下，this 是 undefined。对于对象方法中的内部函数来说，
它不等同于外部函数中的 this，而是取决于内部函数如何调用。如果内部函数是函数调用，那么 this 依然是全局对象或者 undefined。

在方法调用中，this 表示拥有这个方法的对象，这一点和我们的常识相符。如果对象的方法被赋值给一个变量，然后再调用这个变量，此时方法内的 this 还是变为全局对象。解决方案是在赋值时 bind(object)。

构造函数中的 this 表示新创建的对象。应该禁止使用函数调用来创建新对象，因为此时的 “构造函数” 里，this 其实表示全局对象，并不会真的创建新对象。创建新对象时一定要用 new 关键字，同时在构造函数中也可以
判断 this 的类型来做一层保护。

使用 call 或者 apply 是间接调用，第一个参数总是表示函数的调用者。区别在于前者接收多个参数，后者把多个参数放在一个数组中。间接调用中，this 表示第一个参数。

被绑定的函数和原函数享有同样的作用域和代码，但是上下文不同，也就是 this 不同。在绑定的函数中，this 表示 bind 函数的第一个参数。

箭头函数也叫匿名函数（闭包），在箭头函数中，this 表示定义箭头函数的环境中，this 表示的内容。也就是说箭头函数内部的 this 会持有它外部所处环境的 this。如果在全局环境中把某个对象的方法定义为箭头函数，此时
箭头函数中的 this 自然就变成了全局对象。

总的来说，需要判断 this 时，问问自己：“这个函数是怎么调用的？”，如果是箭头函数，问问自己：“这个函数是在哪里定义的？”


