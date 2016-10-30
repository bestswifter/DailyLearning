# 我的 Shell 学习笔记

记录小白学习 Shell 的过程，如有错误，万望指正，感激不尽。

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
		pwd=$(pwd)
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
2. if 判断也要加空格，比如 `if [ $i -eq 1 ]`

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


