### [#65 迷你MVVM](http://scriptoj.mangojuice.top/problems/65)

做一个小型的 MVVM 库，可以做到数据和视图之间的自动同步。

你需要做的就是完成一个函数 `bindViewToData`，它接受一个 DOM 节点和一个对象 `data` 作为参数。`bindViewToData` 会分析这个 DOM 节点下的所有文本节点，并且分析其中被 `{{` 和 `}}` 包裹起来的表达式，然后把其中的内容替换成在 `data `上下文执行该表达式的结果。例如：

```
<div id='app'>
  <p>
    My name is {{firstName + ' ' + lastName}}, I am {{age}} years old.
  </p>
<div>
```

```
const appData = {
  firstName: 'Lucy',
  lastName: 'Green',
  age: 13
}
bindViewToData(document.getElementById('app'), appData)

// div 里面的 p 元素的内容为
// My name is Lucy Green, I am 13 years old.

appData.firstName = 'Jerry'
appData.age = 16

// div 里面的 p 元素的内容自动变为
// My name is Jerry Green, I am 16 years old.
```

当数据改变的时候，会自动地把相应的表达式的内容重新计算并且插入文本节点。

-----
参考答案：

无