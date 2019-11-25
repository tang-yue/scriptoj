### [#72 使用generator模拟async/await](http://scriptoj.mangojuice.top/problems/72)

----
题目描述：

在远古时代，我们使用 callback 进行异步流程控制，但是会有 callback hell 的问题。经过历史的发展，逐渐地使用了不少的工具进行异步流程的改进，例如 Async.js、Promise 等，到后来的 generator + promise，还有最终的方案 async/await。了解以前是用什么方案处理异步流程控制，对我们理解现在的 asyn/await 也是很有好处。

请你实现一个简单的函数 `wrapAsync`，使用 generator + promise 来模拟 async/await 进行异步流程的控制。`wrapAsync` 接受一个 generator 函数作为参数，并且返回一个函数。`generator` 函数内部可以使用关键字 `yield` 一个 Promise 对象，并且可以类似 async/await 那样获取到 Promise 的返回结果，例如：

```
const getData = (name) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('My name is ' + name)
    }, 100) // 模拟异步获取数据
  })
}

const run = wrapAsync(function * (lastName) {
  const data1 = yield getData('Jerry ' + lastName)
  const data2 = yield getData('Lucy ' + lastName)
  return [data1, data2]
})

run('Green').then((val) => {
  console.log(val) // => [ 'My name is Jerry Green', 'My name is Lucy Green' ]
})
```

`getData` 是一个异步函数并且返回 Promise，我们通过 `yield` 关键字获取到这个异步函数的 Promise 返回的结果，在代码编写上起来像是同步的，执行上实际是异步的。

请你完成 `wrapAsync` 的编写，`wrapAsync` 返回的函数接受的参数和传入的 generator 接受的函数保持一致，并且在调用的时候会传给 generator 函数（正如上面的例子）；另外，`wrapAsync` 返回的函数执行结果是一个 Promise，我们可以通过这个 Promise 获取到 generator 函数执行的结果（正如上面的例子）。

（此简单实现你暂时不需要考虑异常的控制。）


----
参考答案：

无