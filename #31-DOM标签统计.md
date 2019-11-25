### [#31 DOM标签统计](http://scriptoj.mangojuice.top/problems/31)

----
题目描述：

完成 `getPageTags` 函数，判断你的代码所执行的页面用到了哪些标签。

例如，如果页面中：

```
<html>
  <head></head>
  <body></body>
  <script>...</script>
</html>
```

那么 `getPageTags()` 则返回数组 `['html', 'head' 'body', 'script']`（顺序不重要）。

----
思路：
如何获取所有的dom， 然后将dom里的所有tagName 都取出来。

----
参考答案：

```js
const getPageTags = () => {
  const doms = document.getElementsByTagName('*');
  return [...new Set([...doms].map((dom) => dom.tagName.toLowerCase()))];
}
```
