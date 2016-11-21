# 我的 Python 学习笔记

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
