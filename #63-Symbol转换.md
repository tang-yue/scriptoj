### [#63 Symbol 转换](http://scriptoj.mangojuice.top/problems/63)

----
题目描述：

请你完成 `convertSymbolToNormalStr` 函数，它会把一个键全是 Symbol 的对象转换成键全是 String 的对象，而同时值保持不变。例如：

```js
convertSymbolToNormalStr({ [Symbol('name')]: 'Jerry' }) // => { name: 'Jerry' }
```

----
思路：

[getOwnPropertySymbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)

----
参考答案：

```js
const convertSymbolToNormalStr = obj => { 
  Object.getOwnPropertySymbols(obj).forEach(symbol => {
    let key = symbol.toString().match(/\((.*)\)/)[1];
    obj[key] = obj[symbol];
    delete obj[symbol]
  })
  return obj;
}
```