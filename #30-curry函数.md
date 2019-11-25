### [#30 curry函数](http://scriptoj.mangojuice.top/problems/30)

----
题目描述：

函数式编程当中有一个非常重要的概念就是 [函数柯里化](https://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96)。一个接受 任意多个参数 的函数，如果执行的时候传入的参数不足，那么它会返回新的函数，新的函数会接受剩余的参数，直到所有参数都传入才执行操作。这种技术就叫柯里化。请你完成 curry 函数，它可以把任意的函数进行柯里化，效果如下：

```
const f = (a, b, c d) => { ... }
const curried = curry(f)

curried(a, b, c, d)
curried(a, b, c)(d)
curried(a)(b, c, d)
curried(a, b)(c, d)
curried(a)(b, c)(d)
curried(a)(b)(c, d)
curried(a, b)(c)(d)
// ...
// 这些函数执行结果都一样

// 经典加法例子
const add = curry((a, b) => a + b)
const add1 = add(1)

add1(1) // => 2
add1(2) // => 3
add1(3) // => 4
```
注意，传给 `curry` 的函数可能会有任意多个参数。

----
思路：

如果你固定某些参数，你将得到接受余下参数的一个函数

----
参考答案：

```
const curry = (fn, arr = []) => {
  return (...args) => {
    if([...arr, ...args].length === fn.length) {
      return fn(...arr, ...args)
    } else {
      return curry(fn, [...arr, ...args])
    }
  }
}
```




