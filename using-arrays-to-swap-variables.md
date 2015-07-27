# 使用数组来交换变量
## 问题
你想通过数组来交换变量。
## 解决方案
使用 CoffeeScript 的解构赋值语法：
```
a = 1
b = 3

[a, b] = [b, a]

a
# => 3

b
# => 1
```
## 讨论
解构赋值可以不依赖临时变量实现变量值的交换。 

这种语法特别适合在遍历数组的时候只想迭代最短数组的情况：
```
ray1 = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
ray2 = [ 5, 9, 14, 20 ]

intersection = (a, b) ->
  [a, b] = [b, a] if a.length > b.length
  value for value in a when value in b

intersection ray1, ray2
# => [ 5, 9 ]

intersection ray2, ray1
# => [ 5, 9 ]
```

