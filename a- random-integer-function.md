# 一个随机整数函数

## 问题

你想要获得两个整数（包含在内）之间的一个随机整数。

## 方案

使用以下的函数。

```
randomInt = (lower, upper) ->
  [lower, upper] = [0, lower]     unless upper?           # 用一个参数调用
  [lower, upper] = [upper, lower] if lower > upper        # Lower 必须小于 upper
  Math.floor(Math.random() * (upper - lower + 1) + lower) # 最后一条语句是一个返回值

(randomInt(1) for i in [0...10])
# => [0,1,1,0,0,0,1,1,1,0]

(randomInt(1, 10) for i in [0...10])
# => [7,3,9,1,8,5,4,10,10,8]
```


