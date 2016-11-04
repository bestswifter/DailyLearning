# 我的 Python 学习笔记

## 2016.11.5

### List 的 in-place 修改

假设需要修改 list 中的每个元素，一种方案是遍历列表然后把取出的元素做修改，最后加到一个新的列表中。一种简单的思路如下

```python
list = [element.replace("a", "b") for element in list]
```
