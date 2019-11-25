### [#93 Virtual DOM](http://scriptoj.mangojuice.top/problems/93)

----
题目描述：

大家都知道，React、Vue 内部都采用了 Virtual DOM 的技术。简而言之，就是用普通的 JavaScript 对象来表示 DOM 的结构和信息。例如下面的 DOM 结构：

```js
<ul id='list' style='color: red'>
  <li class='item'>Item 1</li>
  <li class='item'>Item 2</li>
  <li class='item'>Item 3</li>
</ul>
```
可以用 JavaScript 的对象表示为：

```js
const ul = {
  tagName: 'ul', // 节点标签名
  props: { // DOM的属性，用一个对象存储键值对
    id: 'list',
    style: 'color: red'
  },
  children: [ // 该节点的子节点
    {tagName: 'li', props: {class: 'item'}, children: ["Item 1"]},
    {tagName: 'li', props: {class: 'item'}, children: ["Item 2"]},
    {tagName: 'li', props: {class: 'item'}, children: ["Item 3"]},
  ]
}
```

每个代表 DOM 节点的对象有三个属性：

1、`tagName`：代表 DOM 节点的标签名。
2、`props`：这个 DOM 节点的属性，用一个对象表示。
2、`children`：这个 DOM 节点的子节点，是一个数组；数组的元素可以是 字符串 或者 对象。如果是字符串就表示这个子节点是文本节点，否则就表示是另外一个 DOM 节点。
请你完成 `h` 函数，可以通过 `h` 函数生成上面的结果，例如：

```
const ul = h('ul', {id: 'list', style: 'color: red'}, [
  h('li', {class: 'item'}, ['Item 1']),
  h('li', {class: 'item'}, ['Item 2']),
  h('li', {class: 'item'}, ['Item 3'])
])

ul.props.id === 'list' // => true
```

`h` 函数需要返回的实例是属于 `VNode` 这个类的：

```
ul instanceof VNode // => true
```

请你完成 `h` 函数和 `VNode` 的实现。

----
参考答案：

```js
class VNode {
  constructor(tagName, props, children) {
    this.tagName = tagName;
    this.props = props;
    this.children = children;
  }
}

const h = function (t, p, c) {
  return new VNode(t, p, c)
}
```