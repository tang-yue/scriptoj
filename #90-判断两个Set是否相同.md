### [#90 判断两个Set是否相同](http://scriptoj.mangojuice.top/problems/90)

----
题目描述：

完成 `isSameSet` 函数，它接受了两个 Set 对象作为参数，请你返回 `true/false` 来表明这两个 set 的内容是否完全一致，例如：

```
const a = {}
const b = 1
const c = 'ScriptOJ'

const set1 = new Set([a, b, c])
const set2 = new Set([a, c, b])

isSameSet(set1, set2) // => true
```

---
思路：

1. 首先判断长度是否一致，
2. 之后判断s1里的值是否也都是在s2里

----
参考答案：

```js
const isSameSet = (s1, s2) => {
  if(s1.size !== s2.size) {
    return false;
  }
  return [...s1].every(i => s2.has(i))
}
```