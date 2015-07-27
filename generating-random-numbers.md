# 生成随机数

## 问题

你需要生成在一定范围内的随机数。

## 解决方案

使用 JavaScript 的 Math.random() 来获得浮点数，满足 0<=X<1.0 。使用乘法和 Math.floor 得到在一定范围内的数字。
```
probability = Math.random()
0.0 <= probability < 1.0
# => true

# 注意百分位数不会达到 100。从 0 到 100 的范围实际上是 101 的跨度。
percentile = Math.floor(Math.random() * 100)
0 <= percentile < 100
# => true

dice = Math.floor(Math.random() * 6) + 1
1 <= dice <= 6
# => true

max = 42
min = -13
range = Math.random() * (max - min) + min
-13 <= range < 42
# => true
```
## 讨论

对于 JavaScript 来说，它更直接更快。

需要注意到 JavaScript 的 Math.random() 不能通过发生器生成随机数种子来得到特定值。详情可参考[产生可预测的随机数](http://coffeescript-cookbook.github.io/chapters/math/generating-predictable-random-numbers)。

产生一个从 0 到 n（不包括在内）的数，乘以 n。  
产生一个从 1 到 n（包含在内）的数，乘以 n 然后加上 1。

