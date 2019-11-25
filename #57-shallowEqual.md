### [#57 shallowEqual](http://scriptoj.mangojuice.top/problems/57)

----
题目描述：

在 React、Redux 当中，经常会用到一种 `shallowEqual` 来做性能优化。`shallowEqual` 结合 immutable 的共享数据结构可以帮助我们简单地检测到哪些数据没有发生变化，就不需要做额外的渲染等操作，优化效果拨群。

简单来说，`shallowEqual` 接受两个参数，如果这两个参数的值相同、或者这两个参数都是对象并且对象的第一层数据相同，那么就返回 `true`；否则就返回 `false`。例如：

```
shallowEqual(1, 1) // true
shallowEqual(1, 2) // false
shallowEqual('foo', 'foo') // true
shallowEqual(window, window) // true
shallowEqual('foo', 'bar') // false
shallowEqual([], []) // true
shallowEqual([1, 2, 3], [1, 2, 3]) // true
shallowEqual({ name: 'Jerry' }, { name: 'Jerry' }) // true
shallowEqual({ age: NaN }, { age: NaN }) // true
shallowEqual(null, { age: NaN }) // false

var a = { name: 'Jerry' }
var b = { age: 12 }
shallowEqual({ a, b }, { a, b }) // true
shallowEqual({ name: { a, b } }, { name: { a, b } } // false
shallowEqual({ a, b }, { a }) // false
```

请你完成 `shallowEqual`函数。

----
思路：

必须要保证这两个参数的值相同，或者这两个参数都是对象并且对象的第一层数据相同。
所以我的想法是如何是基本类型，只要判断是否相等就可以了，如果不是基本类型，那么要遍历第一层，
看对应的值是否相等，如果相等则返回true，如果不是则返回false。
但是我发现我这样的思路是不正确的。

遇到的问题：为什么传入参数-0 和 +0 返回的是false呢

[Object.is()是做什么的](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

Object.is() 并不会特殊处理NaN,-0,+0

----
参考答案：

```js
const shallowEqual = (x, y) => {
  if (typeof x !== typeof y) return false
  if (typeof x !== 'object') {
    return Object.is(x, y)
  } else {
    if (x === null || y === null) return x === y
    let keys1 = Object.keys(x), keys2 = Object.keys(y)
    if (keys1.length !== keys2.length) return false
    return keys1.every(d => Object.is(x[d], y[d]))
  }
}
```