# Java 学习日记

## 2017-03-16

### 字符串截取

```java
String s = "abcdefghijklmno";
String t = s.substring(0, 10); // 前十个字符
```

## 2017-01-17

### 字符串与数字互转

* 字符串转数字: Integer.parseInt(string)，注意 Try-Catch
* 数字转字符串：String.valueOf(int)

## 2017-01-09

### @override

在 Java 6 以后，实现接口方法时，可以加上 @override 来提前发现不必要的错误。但是 Java 5 及其之前的版本不允许这么做
