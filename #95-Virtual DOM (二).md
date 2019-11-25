### [#95 Virtual DOM (二)](http://scriptoj.mangojuice.top/problems/95)

----
题目描述：

在[Virtual DOM](http://scriptoj.mangojuice.top/problems/93)的基础上给VNode类添加`render`方法,`render`方法把一个虚拟的DOM节点渲染成真正的 DOM 节点，例如：

```
const ul = h('ul', {id: 'list', style: 'color: red'}, [
  h('li', {class: 'item'}, ['Item 1']),
  h('li', {class: 'item'}, ['Item 2']),
  h('li', {class: 'item'}, ['Item 3'])
]

const urlDom = ul.render() // 渲染 DOM 节点和它的子节点
ulDom.getAttribute('id') === 'list' // true
ulDom.querySelectorAll('li').length === 3 // true
```

----
参考答案：

```js
class VNode {
  constructor(tagName, props, children) {
    this.tagName = tagName;
    this.props = props;
    this.children = children;
  }

  render() {
    // 创建 LI 标签元素
    const dom = document.createElement(this.tagName);
    if(this.props) {
      const props = Object.keys(this.props);
      props.map(prop => {
        dom.setAttribute(prop, this.props[prop])
      })
    }

    if(this.children) {
      this.children.map(child => {
        dom.appendChild(child instanceof VNode ? child.render() : document.createTextNode(child));
      })
    }
    return dom;
  }
}

const h = (tagName, props, children) => new VNode(tagName, props, children);
```