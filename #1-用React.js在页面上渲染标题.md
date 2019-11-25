### [#1 用React.js在页面上渲染标题](http://scriptoj.mangojuice.top/problems/1)

----
题目描述：

在页面上增加一个 `id`为 `root` 的 `<div>` 元素。然后请你完成一个 `renderContent` 函数，这个函数会把传入的任意字符串都包装到一个 `<h1>` 元素中并且渲染到页面上。例如：

```js
renderContent('Hello World')
```

页面上就有相应的内容：

```js
<div id='root'>
  <h1>Hello World</h1>
</div>
```

只能通过 React.js 来实现。

----
参考答案：

HTML：
```
<div id='root'>
  <h1>renderContent('Hello World')</h1>
</div>
```

js：
```js
function renderContent (content) {
  ReactDOM.render(<h1>{content}</h1>, document.getElementById('root'))
}
```



