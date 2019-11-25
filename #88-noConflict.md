### [#88 noConflict](http://scriptoj.mangojuice.top/problems/88)

----
题目描述

很多第三方库都会往自己的全局环境中注入一些便捷的全局变量，例如 `$` 和 `_`。但是如果两个库都使用了 `$` 就会有命名冲突的问题。所以类似 jQuery 会自带一个 `noConflict` 方法，来防止这种命名的冲突。`noConflict` 会恢复原来的 `$` 全局变量，并且返回自己原本应该注入的变量。例如：

```
// 假设 $ 已经定义为 'ScriptOJ'

// 加载 jQuery，$ 变量被覆盖，变成了 jQuery。
// ...
$('body').html('ScriptOJ')

// noConflict 恢复原来的 $ 变量
const j = $.noConflict()
$ === 'ScriptOJ' // => true

// 现在 jQuery 变成了 j
j('body').html('Hello')
```

请你完成一个立即执行的函数，这个函数会往全局变量中注入 `$` 对象变量，并且 `$`有一个 `noConflict` 方法来达到上面的效果。

----
参考答案：

无
