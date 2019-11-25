### [#83 filter map](http://scriptoj.mangojuice.top/problems/83)

----
题目描述：

请你给原生的 Map 添加方法 `filterKeys` 和 `filterValues`，可以类似于于数组方法的 `filter`。它们分别可以对 Map 的键和值进行筛选，它们会返回一个新的 Map， 是对原有的 Map 的筛选结果，例如：

```
const m = new Map([['Jerry', 12], ['Jimmy', 13], ['Tomy', 14]])

m.filterKeys((key) => key.startsWith('J')) // => Map { Jerry => 12, Jimmy => 13 }
m.filterValues((val) => val >= 13) // => Map { Jimmy => 13, Tomy => 14  }

// 原有的 map 保持不变
console.log(m) // => Map { Jerry => 12 , Jimmy => 13, Tomy => 14 }
```

----
参考答案：

```js
Map.prototype.filterKeys = function(fn) {
  return new Map([...this].filter(([k, v]) => fn(k)));
}


Map.prototype.filterValues = function(fn) {
  return new Map([...this].filter(([k, v]) => fn(v)));
}
```
