# 查找子字符串

## 问题

你想在一条消息中查找某个关键字第一次或最后一次出现的位置。

## 解决方案

分别使用 JavaScript 的 indexOf() 和 lastIndexOf() 方法查找字符串第一次和最后一次出现的位置。语法: string.indexOf searchstring, start

```
message = "This is a test string. This has a repeat or two. This might even have a third."
message.indexOf "This", 0
# => 0

# Modifying the start parameter
message.indexOf "This", 5
# => 23

message.lastIndexOf "This"
# => 49
```

## 讨论

还需要想办法统计出给定字符串在一条消息中出现的次数。
