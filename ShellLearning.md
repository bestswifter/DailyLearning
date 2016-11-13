# 我的 Shell 学习笔记

记录小白学习 Shell 的过程，如有错误，万望指正，感激不尽。

## 2016.11.13

### uniq

`uniq` 命令可以过滤掉输出结果中同样的内容，如果加上 `-c` 参数，会在输出结果的前面加上一个数字，表示这个内容在原内容中输出多少次，如果稍后使用 `sort -n`，即可进行排序

## 2016.11.6

### xargs 简介

文档中的定义：

> The xargs utility reads space, tab, newline and end-of-file delimited strings from the standard input and executes utility with the strings as arguments.

对于某些命令，比如 grep 和 awk，可以从管道里面读取标准输入作为要处理的内容，比如 `ls | grep "..."`，这些命令也可以单独指定从某个文件里读取内容，比如 `grep "..." file`。

对于其他有些命令，比如 cp、mv、rm 等，只能手动输入参数，比如 `rm -rf dictory`，这里的文件夹名称不可省略，也不能从管道读取。但实际上我们有时候希望它能使用上一个命令的输出结果。

这就是 xargs 命令的作用，它用来处理标准输入中的内容，经过一定的加工处理，然后调用别的服务。由于它是按行读取并调用，所以另一个好处是避免参数过大导致无法处理。

考虑这个命令：

```bash
ls | grep "a"
```

`grep` 命令首先会读取 ls 命令的所有输出内容，然后搜索其中含有字母 a 的内容。整个命令相当于是查找文件名中包含 a 的文件。

再考虑下面这个命令：

```bash
ls | xargs grep "a"
```

xargs 会读取标准输入流中的内容，将空格、tab、换行符等作为分隔符，这样就把标准输入分割为很多份，对于每一份都调用后面的服务，并且将分割结果作为参数插入到命令的最后。

在这个例子中，我们相当于执行了一个 for 循环：

```python
for argument in result_of_command(ls):
    grep "a" argument
```

显然，这个命令是在所有 ls 的输出结果中，去文件里查找内容。

xargs 还有两个常见进阶使用：

1. 合并多个参数，假设 ls 的输出结果，每一行有两个单词，如果直接使用，他们会分别被传入后续命令中，如果想要把他们合并成一个字符串，使用 `xargs -n 2`，表示每次合并三个参数。
2. xargs 默认是将分割出的字符串插入到下一个命令的末尾，对于 mv 这样参数在中间的命令，就无法支持了，解决方案是使用 -I 后面接一个占位符：

```bash
ls | xargs -I file mv file ~/Desktop
```

参考资料：[xargs: How To Control and Use Command Line Arguments](http://www.cyberciti.biz/faq/linux-unix-bsd-xargs-construct-argument-lists-utility/)、[10 Xargs Command Examples in Linux / UNIX](http://www.thegeekstuff.com/2013/12/xargs-examples/)

### awk 能做什么

1. 文本统计，可以统计单词数（就是 field 的数量）、总字母数（用字符串的 `length` 方法）、计算总行数等，而且不耗内存
2. 打印指定行数（通过 NR 来过滤）

## 2016.11.5

### tee 入门

tee 命令可以将标准输入流中的内容复制到标准输出流或文件中， 通常可以用来在屏幕显示输出内容的同时，将内容输出到标准输出流中，这样管道的下一个程序还可以接着使用。比如：

```bash
ls | tee /dev/tty | sed ......
```

### awk 入门

awk 也是一个文件处理工具，基本模式是：`awk 'pattern {action}'` 表示逐行读取输入内容，检测是否匹配上 patter，如果能匹配则执行 action。

awk 最好用单引号包裹命令，这样可以避免 `$` 被 shell 转义，也可以输入多行命令（言下之意双引号不具备这些特性？）

如果行数比较多，可以把单引号中的内容（不含单引号）写到一个文件，比如叫 progfile，然后执行 `awk -f progfile 输入文件`

一些小技巧：

1. 可以定义变量，而且不用预先声明，默认值为 0
2. 可以用 BEGIN 和 END 来匹配文章的开头和末尾，从而提供额外的输出
3. 可以使用 `printf` 函数，用法和 C 语言完全一致
4. 字符串可以进行拼接， 比如 a = b " " c，把需要被拼接的内容用空格隔开即可

### sed 入门

sed 用来处理文件，它依次读取文件的每一行，做处理，然后输出。如果想要改变原来的文件，可以添加 `-i` 参数表示写回原文件。

语法如下：

```bash
sed "地址/操作符/Pattern/新的值/命令 file"
```

最简单的例子：

```bash
sed "s/a/b/g" file
```

这行命令表示把文件 file 中每一行里的 `a` 替换成 `b`，其中 s 表示替换，g 表示替换所有的 a (不写 g 的话只会替换第一个 a)。它其实用了 `sed "操作符/Pattern/新的值/命令"` 这种模式。

地址表示你要操作的地址，比如 `sed "1,2/s/a/b/g" file` 表示只对 1-2 行的内容进行上述操作。

地址也可以用正则表达式，比如 `sed "/begin/,/end/s/a/b/g" file` 表示开始行能够匹配 begin，结尾行能够匹配 end，并在这个范围内进行上述操作。

有一个比较高级的功能，在匹配 pattern 时，可以用圆括号把正则表达式包起来，这样稍后就可以当做变量来用。比如：

```bash
sed "s/\(ab\)/\1/g" file
```

这里的正则表达式 `ab` 被写在圆括号里，所以 `\1` 就表示它能匹配的内容。

sed 的使用，归根结底还是正则表达式的使用，关于 sed 自身的功能，反而并不是非常复杂（至少应付日常工作足够了）

## 2016.10.31

### mdfind

mdfind 命令可以用来搜索文件，或者文件中的内容，和 grep 有一定的类似。但是即使是搜索文件内容，也只能显示包含这个内容的文件名。

基本语法：

```bash
mdfind -onlyin 搜索目录 -name 文件名
```

## 2016.10.30

### bash 函数入门

不用指定参数类型和个数，直接用 `$0`, `$1` 等来表示。`$#` 表示函数个数。

如果想获得某个函数的执行结果（比如 pwd），可以写为 `pwd=$(pwd)`。

以自定义的 `bsgrep` 函数为例演示：

```bash
bsgrep () {
	if [ $# -eq 1 ]
	then
		grep --color=auto --exclude-dir={.bzr,CVS,.git,.hg,.svn} -rna "$1" .
	else
		grep --color=auto --exclude-dir={.bzr,CVS,.git,.hg,.svn} -na "$1" "$pwd/$2"
	fi
}
```

### grep 的使用（续）

基本用法是 `grep content file` 或者 `grep -r content directory`。

搜索的时候，遇到有些字符串需要转义。比如 `a*b` 会被当做正则来处理（0 到多个 a 后面加上一个 b），如果想搜索字面量，需要用 `a\*b`。

搜索连字符 `-` 也是同理，比如文本 `-n` 需要用 `\-n` 来处理。

### Shell 中的一些坑

1. 声明变量的时候不能有空格，比如 `a=1` 合法，`a = 1` 不合法
2. if 判断也要加空格，比如 `if [ $i -eq 1  ]`

### Bash 与 zsh 的冲突

在 Mac 上，`echo -n` 表示不换行输出，但是大部分 iTerm 采用了 zsh，所以还是会强制换行，解决方案是执行 `unsetopt PROMPT_CR`

参考地址：[Removing trailing new line after printing on Mac OS X zsh](http://stackoverflow.com/questions/18213751/removing-trailing-new-line-after-printing-on-mac-os-x-zsh)

## 2016.10.27

### zip、unzip

zip 命令可以将文件夹打包，格式：

`zip -r zip_filename.zip directory_want_to_zip`

表示将 directory_want_to_zip 这个文件夹打包成 zip_filename.zip 文件。

unzip 命令用来解压缩 zip 文件，用法：

`unzip -o zipped_filename.zip -d directory_want_to_zip`

表示将 zipped_filename.zip 文件解压到 directory_want_to_zip 目录下，-d 用于指定解压目录，-o 表示强制覆盖同名文件。

## 2016.10.24

### pushd、popd、dirs

`pushd` 的作用是前往参数目录，并将参数目录压入栈中。`popd` 则是将栈顶目录弹出并前往该目录。通过 `dirs` 可以查看目录栈结构。

不带参数的 `pushd` 默认前往上一个目录，这也会导致目录栈最上方的两个目录做交换。

## 2016.10.16

### mkdir -p

在某个空目录下，以下命令合法：

```shell
mkdir -p parent/child
```

表示可以创建一个多层次的目录，如果用 `mkdir` 会报错不存在 parent 目录
