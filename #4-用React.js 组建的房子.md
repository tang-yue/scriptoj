### [#4 用React.js组件的房子](http://scriptoj.mangojuice.top/problems/4)

----
题目描述：

一个房子里面有一个房间和一个洗手间，房间里面有一个人和两条狗。

请你完成组件：`House`，`Room`，`Bathroom`，`Man`，`Dog`，它们的最外层都用 `div` 标签包裹起来，类名分别为：`house`，`room`，`bathroom`，`man`，`dog`。

组件的实现应该具有上述的嵌套关系。

----
参考答案：

```js
// Component 已经可以直接使用

class House extends Component {
  render() {
    return <div className="house">
    <Room />
    <Bathroom />
    </div>
  }
}

class Room extends Component {
  render() {
    return <div className="room">
    <Man />
    <Dog />
    <Dog />
    </div>
  }
}

class Bathroom extends Component {
  render() {
    return <div className="bathroom"></div>
  }
}

class Man extends Component {
  render() {
    return <div className="man"></div>
  }
}

class Dog extends Component {
  render() {
    return <div className="dog"></div>
  }
}

```