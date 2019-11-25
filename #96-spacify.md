### [#96 spacify](http://scriptoj.mangojuice.top/problems/96)

----
题目描述：

请你给字符串都添加上原型方法 `spacify`，可以让一个字符串的每个字母都多出一个空格的间隔：

```
"ScriptOJ".spacify() // => "S c r i p t O J"
```

----
参考答案：

```js
String.prototype.spacify = function () {
  return this.split('').join(' ')
}
```
