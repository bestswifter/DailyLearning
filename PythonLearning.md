# 我的 Python 学习笔记

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
