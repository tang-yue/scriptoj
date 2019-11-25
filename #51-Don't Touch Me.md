### [#51 Don't Touch Me](http://scriptoj.mangojuice.top/problems/51)

----
题目描述

Tomy 非常敏感，不喜欢别人碰他的东西。一旦有人碰他就会大喊 `Don't Touch Me.`。

完成 `tomy` 这个对象，禁止对 `tomy` 的内容进行修改（增加、修改、删除）。一旦有人对 `tomy` 进行任何的修改，都用 `console.log` 打印 `Don't Touch Me.`。

----
思路：

可用es6的Proxy解此题，Proxy可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截。

----
参考答案：

```
const tomy = new Proxy({}, {
  set: (target, key, value, receiver) => {
    console.log("Don't Touch Me.");
  },
  deleteProperty: (target, propKey) => {
    console.log("Don't Touch Me.")
  }
})
```

