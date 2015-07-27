# 指数对数运算

## 问题

你需要进行包含指数和对数的运算。

## 解决方案

使用 JavaScript 的 Math 对象来提供常用的数学函数。

```
# Math.pow(x, y) 返回 x^y
Math.pow(2, 4)
# => 16

# Math.exp(x) 返回 E^x ，被简写为 Math.pow(Math.E, x)
Math.exp(2)
# => 7.38905609893065

# Math.log returns the natural (base E) log
Math.log(5)
# => 1.6094379124341003
Math.log(Math.exp(42))
# => 42

# To get a log with some other base n, divide by Math.log(n)
Math.log(100) / Math.log(10)
# => 2
```

## 讨论

若想了解关于数学对象的更多信息，请参阅 [Mozilla 开发者网络](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)上的文档。另可参阅[数学常量](http://coffeescript-cookbook.github.io/chapters/math/constants)关于数学对象中各种常量的讨论。

