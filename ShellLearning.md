# 我的 Shell 学习笔记

记录小白学习 Shell 的过程，如有错误，万望指正，感激不尽。

## 2016.10.30

### Shell 中的一些坑

1. 声明变量的时候不能有空格，比如 `a=1` 合法，`a = 1` 不合法
2. if 判断也要加空格，比如 `if [ $i -eq 1 ]`

### Bash 与 zsh 的冲突

在 Mac 上，`echo -n` 表示不换行输出，但是大部分 iTerm 采用了 zsh，所以还是会强制换行，解决方案是执行 `unsetopt PROMPT_CR`

参考地址：[Removing trailing new line after printing on Mac OS X zsh](http://stackoverflow.com/questions/18213751/removing-trailing-new-line-after-printing-on-mac-os-x-zsh)

## 2016.10.16

### mkdir -p

在某个空目录下，以下命令合法：

```shell
mkdir -p parent/child
```

表示可以创建一个多层次的目录，如果用 `mkdir` 会报错不存在 parent 目录

## 2016.10.24

### pushd、popd、dirs

`pushd` 的作用是前往参数目录，并将参数目录压入栈中。`popd` 则是将栈顶目录弹出并前往该目录。通过 `dirs` 可以查看目录栈结构。

不带参数的 `pushd` 默认前往上一个目录，这也会导致目录栈最上方的两个目录做交换。
