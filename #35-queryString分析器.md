### [#35 queryString分析器](http://scriptoj.mangojuice.top/problems/35)

----
题目描述：

在开发当中，我们经常要处理 url。而 url 上的 query string 是我们重点要处理的对象，完成一个 `parseQueryString` 函数。它接受一个 url 字符串作为参数，返回一个对象，这个对象包含 query string 上的键值对。例如：

```
parseQueryString('https://scriptoj.com/problems?offset=100&limit=10')
```
返回：

```
{ offset: '100', limit: '10'}
```


特殊情况说明：如果出现 `?name=&age=12` 则返回 `{ name: '', age: '12' }`，如果 `?name&age=12` 则返回 `{ name: null, age: '12' }`。

请考虑清楚 query string 可能出现的各种情况，包括可能的出现 hash 的情况（`?name=jerry#nice`）。

如果需要帮助，可以对照参 [URI.js](http://medialize.github.io/URI.js/) 的执行结果。


----
参考答案：

```
const parseQueryString = (url) => {
  var obj = {};
  let index = url.indexOf('#');
  url = index === -1 ? url : url.slice(0, index);
  if(url.indexOf('?') !== -1) {
    let i = url.indexOf("?");
    let arr = url.slice(i+1).split('&');
    arr.forEach(function(item)  {
      item = item.replace(/\=/, '&')
      let t = item.split('&');
      obj[t[0]] = t[1] !== undefined ? t[1] : null;
    })
    return obj;     
  } else {
    return obj;
  }
}
```

