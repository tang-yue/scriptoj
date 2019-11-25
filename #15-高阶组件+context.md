### [#15 高阶组件 + context](http://scriptoj.mangojuice.top/problems/15)

----
题目描述：

完成高阶组件 `makeProvider`，接受一个任意类型的数据和组件作为参数：

```js
Post = makeProvider({ name: 'Jerry' })(Post)
```

`Post`下的所有子组件都可以通过 `this.context.data` 获取到传给 `makeProvider` 的参数。如上面的 `Post` 及其子组件的内部可以通过 `this.context.data.name` 获取到 `Jerry`。


----
参考答案：

```js
const makeProvider = (data) => (Post) => {
  return class WrapComponent extends Component {
    static childContextTypes = {
      data: PropTypes.any
    }

    getChildContext() {
      return {
        data: data
      }
    }

    constructor(props) {
      super(props)
    }
    
    render() {
      return (
          <Post />
        )
    }
  }
}
```