### [#17 React-redux实现用户列表的显示、增加、删除](http://scriptoj.mangojuice.top/problems/17)

----
题目描述：

直接使用在 实现 [Users Reducer](http://scriptoj.mangojuice.top/problems/16) 中实现的 userReducer。用 `react-redux` 完成 `UserList`、`User` 组件，可以对用户列表进行显示、增加、删除操作。

你不需要实现 `store` 的生成和使用 `Provider`，只需要完成 `connect` 的过程和组件的实现。

（留意 `<input type="number" />` 的字符串和数字的转换问题）


----
参考答案：


```js
const usersReducer =(state=[],action) => {
  switch(action.type){
    case 'ADD_USER':
      return [...state, action.user]
    case 'DELETE_USER':
      return [...state.slice(0,action.index), ...state.slice(action.index+1)]
    case 'UPDATE_USER':
      let upState = [...state];
      upState[action.index] = {...upState[action.index], ...action.user};
      return upState;
    default:
      return state
  }
}

class User extends Component {
  render () {
    const { user, deleteUser, index } = this.props;
    return (
      <div>
        <div>Name: {user.username}</div>
        <div>Age: {user.age}</div>
        <div>Gender: {user.gender}</div>
        <button onClick={() => {deleteUser(index)}}>删除</button>
      </div>
      )
  }
}

class UsersList extends Component {
  constructor() {
    super();
    this.state = {
      username: '',
      age: '',
      gender: ''
    }
  }

  render() {
    const { username, age, gender } = this.state;
    const { addUser, deleteUser, users } = this.props
    return (
      <div>
        <div className="add-user">
          <div>Username:<input type='text' value={username} onChange={e => {this.setState({username: e.target.value})}} /></div>
          <div>Age: <input type="number" value={age} onChange={e => {this.setState({age: parseInt(e.target.value)})}} /></div>
          <div>Gender:
            <label>Male: <input type="radio" name='gender' value='male' onChange={e => {this.setState({gender: e.target.value})}} /></label>
            <label>Female: <input type="radio" name="gender" value="female" onChange={e => {this.setState({gender: e.target.value})}} /></label>
          </div>
          <button onClick={() => addUser(this.state)}>增加</button>
        </div>
        <div className="users-list">
          {users.map((user, index) =>
            <User user={user} deleteUser={deleteUser} index={index} key={index} />
          )}
        </div>
      </div>
      )
  }
}

// 需要注入的props
const mapStateToProps = (state) => {
  return {
    users: state
  }
}

// 需要注入的函数
const mapDispatchToProps = (dispatch) => {
  return {
    addUser: (user) => {
      dispatch({
        type: 'ADD_USER',
        user
      })
    },
    deleteUser: (index) => {
      dispatch({
        type: 'DELETE_USER',
        index: index
      })
    }
  }
}

UsersList = connect(
  mapStateToProps,
  mapDispatchToProps
  )(UsersList)
```