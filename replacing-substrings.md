# 替换子字符串

## 问题

你需要用另一个值替换字符串的一部分。

## 方案

使用 JavaScript 的 **replace** 方法。它与给定字符串匹配，并返回已编辑的字符串。

第一个版本需要 2 个参数：*模式*和*字符串替换*

```
"JavaScript is my favorite!".replace /Java/, "Coffee"
# => 'CoffeeScript is my favorite!'

"foo bar baz".replace /ba./, "foo"
# => 'foo foo baz'

"foo bar baz".replace /ba./g, "foo"
# => 'foo foo foo'
```

第二个版本需要 2 个参数：*模式*和*回调函数*

```
"CoffeeScript is my favorite!".replace /(\w+)/g, (match) ->
  match.toUpperCase()
# => 'COFFEESCRIPT IS MY FAVORITE!'
```

每次匹配度需要调用回调函数，并且匹配值作为参数传给回调。

## 讨论

正则表达式是一种强有力的方式来匹配和替换字符串。

