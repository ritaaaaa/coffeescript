# 去抖动函数

## 问题

你想只执行一个函数一次，在开始或结束把多个连续的调用合并成一个简单的操作

## 方案

使用一个命名函数

```
debounce: (func, threshold, execAsap) ->
  timeout = null
  (args...) ->
    obj = this
    delayed = ->
      func.apply(obj, args) unless execAsap
      timeout = null
    if timeout
      clearTimeout(timeout)
    else if (execAsap)
      func.apply(obj, args)
    timeout = setTimeout delayed, threshold || 100
mouseMoveHandler: (e) ->
  @debounce((e) ->
    # 只能在鼠标光标停止 300 毫秒后操作一次。
  300)

someOtherHandler: (e) ->
  @debounce((e) ->
    # 只能在初次执行 250 毫秒后操作一次。
  250, true)
```

## 讨论

可参阅 John Hann 的优秀博客文章，了解 [JavaScript 去抖动方法](http://unscriptable.com/2009/03/20/debouncing-javascript-methods/)。

