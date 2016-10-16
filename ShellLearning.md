# 我的 Shell 学习笔记

记录小白学习 Shell 的过程，如有错误，万望指正，感激不尽。

## 2016.10.16

### mkdir -p

在某个空目录下，以下命令合法：

```zsh
mkdir -p parent/child
```

表示可以创建一个多层次的目录，如果用 `mkdir` 会报错不存在 parent 目录
