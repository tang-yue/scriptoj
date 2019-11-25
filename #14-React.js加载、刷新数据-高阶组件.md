### [#14 React.js 加载、刷新数据 - 高阶组件](http://scriptoj.mangojuice.top/problems/14)

----
题目描述：

题目 [#10 React.js 加载、刷新数据](http://scriptoj.mangojuice.top/problems/10) 这种加载数据、刷新数据的行为很常见，现在把这种逻辑抽离到高阶组件当中去。完成高阶组件 `loadAndRefresh`，它具有以下功能：

```js
class Post extends Component {
  render () {
    return (
      <div>
        <p>{this.props.content}</p>
        <button onClick={() => this.props.refresh()}>刷新</button>
      </div>
    )
  }
}

Post = loadAndRefresh('/post')(Post)
```

高阶组件 `loadAndRefresh` 接受一个 `url`作为参数，然后返回一个接受组件作为参数的函数，这个新函数返回一个新的组件。新的组件渲染的时候会给传入的组件设置 `content` 和 `refresh` 作为 `props`。

环境中已经定义好了一个 `getData(url)` 的返回 `Promise` 的函数，它会去服务器取一个字符串的文本，你需要通过 `content` 的 `props` 把它传给被包裹的组件。组件在第一次加载还有 `refresh` 的时候会去服务器取数据。

另外，组件在加载数据的时候，`content`显示 `数据加载中...`。而且，所有传给 `loadAndRefresh` 返回的组件的 `props` 要原封不动传给被包裹的组件。


----
参考答案：

```js
const loadAndRefresh = (url) => {
  return function(WrappedComponent) {
    return class extends Component {
      componentWillMount() {
        this._loadData()
      }
      
      async _loadData() {
        this.setState({ content: '数据加载中...' })
        const content = await getData(url)
        this.setState({
          content,
        })
      }
      render() {
        const props = {
          content: this.state.content,
          refresh: this._loadData.bind(this),
          ...this.props
        }
        return (
            <WrappedComponent {...props} />
          )
      }
    }
  }
}
```


