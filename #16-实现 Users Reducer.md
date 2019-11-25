### [#16 实现 User Reducer](http://scriptoj.mangojuice.top/problems/16)

----
题目描述：

完成一个符合 Redux 要求的 Reducer `usersReducer`，它可以支持以下对一个存储用户信息的数组进行增、删、改的需求：

```js
/**
 * 用户的数据包括三部分，姓名（username）、年龄（age）、性别（gender）
 */

/* 增加用户操作 */
dispatch({
  type: 'ADD_USER',
  user: {
    username: 'Lucy',
    age: 12,
    gender: 'female'
  }
})

/* 通过 id 删除用户操作 */
dispatch({
  type: 'DELETE_USER',
  index: 0 // 删除特定下标用户
})

/* 修改用户操作 */
dispatch({
  type: 'UPDATE_USER',
  index: 0,
  user: {
    username: 'Tomy',
    age: 12,
    gender: 'male'
  }
})
```

修改用户数据的时候，可以进行部分地修改，而不是完全地替换。

注意，`usersReducer` 的 `state` 就是一个数组，你不需用把它包装到一个对象当中。

----
参考答案：

```js
const usersReducer = (state=[], action) => {
  switch(action.type) {
    case "ADD_USER":
      return [...state, action.user];
    case "DELETE_USER":
      let deState = [...state];
      deState.splice(action.index, 1);
      return deState;
    case "UPDATE_USER":
      let upState = [...state];
      upState[action.index] = {...upState[action.index], ...action.user};
      return upState;
    default:
      return state;
  }
}
```

