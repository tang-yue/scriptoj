### [#34 操作 COOKIE](http://scriptoj.mangojuice.top/problems/34)

----
题目描述：

完成 `cookieJar` 单例，它有三个方法：

- `set(name, value, days)`：设置 `cookie` 的值，`days` 为多少天以后过期。
- `get(name)`：获取 `cookie` 的值。
- `remove(name)`：删除 `cookie` 的值。


---
思路：

纯记忆力的东西

----
参考答案：

```
const cookieJar = {
  set (name, value, days) {
    const d = new Date();
    d.setDate(d.getDate() + days)
    document.cookie = `${name}=${value};expires=${d}`
  },
  get (name) {
    const obj = {};
    document.cookie.split(';').forEach(s => {
      s = s.trim();
      const key = s.slice(0, s.indexOf('='));
      const value = s.slice(s.indexOf('=') + 1);
      obj[key] = value;
    })
    return obj[name];
  },
  remove (name) {
    this.set(name, '', -1)
  }
}
```
