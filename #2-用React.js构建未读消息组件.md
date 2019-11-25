### [#2 用React.js构建未读消息组件](http://scriptoj.mangojuice.top/problems/2)

----
题目描述：

使用 React.js 构建一个未读消息组件 `Notification`。

通过 `getNotificationsCount()` 来获取未读消息的数量 ，如果有未读消息 `N` 条，而且 `N > 0`，那么 `Notification` 组件渲染显示：
a

```
<span>有(N)条未读消息</span>
```

否则显示：

```
<span>没有未读消息</span>
```

----
参考答案：

```js
class Notification extends Component {
  render () {
    // TODO
    let N = getNotificationsCount();
    return <div>{
    N > 0 ? <span>有({N})条未读消息</span>
    : <span>没有未读消息</span>}</div>
  }
}
```
