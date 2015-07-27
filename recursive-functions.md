# 递归函数

## 问题

你想在一个函数中调用相同的函数。

## 解决方案

使用一个命名函数：

```
ping = ->
    console.log "Pinged"
    setTimeout ping, 1000
```

若为未命名函数，则使用 @arguments.callee@：

```
delay = 1000

setTimeout((->
    console.log "Pinged"
    setTimeout arguments.callee, delay
    ), delay)
```

## 讨论
虽然 **arguments.callee** 允许未命名函数的递归，在内存密集型应用中占有一定优势，但是命名函数相对来说目的更加明确，也更易于代码的维护。

