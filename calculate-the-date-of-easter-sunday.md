# 计算复活节的日期
##　问题
你需要在给出的年份中找到复活节的月份和日期。
##　解决方案
下面的函数返回数组有两个要素：复活节的月份（ 1-12 ）和日期。如果没有给出任何参数，给出的结果是当前的一年。这是 在CoffeeScript 的[匿名公历算法](https://en.wikipedia.org/wiki/Computus#Anonymous_Gregorian_algorithm)实现的。
```
gregorianEaster = (year = (new Date).getFullYear()) ->
  a = year % 19
  b = ~~(year / 100)
  c = year % 100
  d = ~~(b / 4)
  e = b % 4
  f = ~~((b + 8) / 25)
  g = ~~((b - f + 1) / 3)
  h = (19 * a + b - d - g + 15) % 30
  i = ~~(c / 4)
  k = c % 4
  l = (32 + 2 * e + 2 * i - h - k) % 7
  m = ~~((a + 11 * h + 22 * l) / 451)
  n = h + l - 7 * m + 114
  month = ~~(n / 31)
  day = (n % 31) + 1
  [month, day]
```
##　讨论  
Javascript 中的月份是 0-11 。 getMonth() 查找的是三月的话将返回数字 2 ，这个函数会返回 3 。如果你想要这个功能是一致的，你可以修改这个函数。 

该函数使用~~符号代替来 Math.floor() 。
```
gregorianEaster()    # => [4, 24] (April 24th in 2011)
gregorianEaster 1972 # => [4, 2]
```

