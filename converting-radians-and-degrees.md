# 转换弧度和度

## 问题

你需要实现弧度和度之间的转换。

## 方案

使用 JavaScript 的 Math.PI 和一个简单的公式来转换两者。

```
# 弧度转换成度
radiansToDegrees = (radians) ->
    degrees = radians * 180 / Math.PI

radiansToDegrees(1)
# => 57.29577951308232

# 度转换成弧度
degreesToRadians = (degrees) ->
    radians = degrees * Math.PI / 180

degreesToRadians(1)
# => 0.017453292519943295
```

