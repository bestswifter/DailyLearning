# 我的 Python 学习笔记

## 2016.11.28

### __getattr__ and __setattr__

这两个方法可以拦截对 python 类对象的读取和写入操作，相当于自定义了任意 `attribute` 的 setter 和 getter。

## 2016.11.25

### json

要解析或生成 json 文件，首先要导入 json 模块，然后使用`load(s)` 和 `dump(s)` 方法。

带有 `s` 的方法表示这个方法操作的是字符串，否则操作文件描述符

## 2016.11.24

### os.walk

`walk` 方法用来遍历某个目录，它返回一个三元 tuple 的数组，每个 tuple 由三个部分组成：

1. path: 路径名
2. dirnames：该子路径下的所有文件夹的名字
3. filenames：改子路径下的所有文件的名字

## 2016.11.21

### 函数参数

`*args`是可变参数，args接收的是一个tuple；`**kw`是关键字参数，kw接收的是一个dict。

可变参数既可以直接传入：`func(1, 2, 3)`，又可以先组装list或tuple，再通过 `*args`传入：`func(*(1, 2, 3))`；

关键字参数既可以直接传入：func(a=1, b=2)，又可以先组装dict，再通过`**kw`传入：`func(**{'a': 1, 'b': 2})`。

## 2016.11.16

### 解析 JSON

```python
import json
f = open(fiel_name, 'r')
json_content = json.loads(f.read())
value = json_content[path1][path2]
```

### 遍历数组

除了 `for x in array` 这种写法以外，还可以使用：

```python
for index, element in enumerate(array): 
```

好处是可以拿到 index

## 2016.11.5

### List 的 in-place 修改

假设需要修改 list 中的每个元素，一种方案是遍历列表然后把取出的元素做修改，最后加到一个新的列表中。一种简单的思路如下

```python
list = [element.replace("a", "b") for element in list]
```
