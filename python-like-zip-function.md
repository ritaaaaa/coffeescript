# 类似 Python 的 zip 函数
## 问题
你想把多个数组连在一起，生成一个数组的数组。换句话说，你需要实现与 Python 中的 zip 函数类似的功能。 Python 的 zip 函数返回的是元组的数组，其中每个元组中包含着作为参数的数组中的第 i 个元素。
## 解决方案
使用下面的 CoffeeScript 代码：
```
# Usage: zip(arr1, arr2, arr3, ...)
zip = () ->
  lengthArray = (arr.length for arr in arguments)
  length = Math.max.apply(Math, lengthArray)
  argumentLength = arguments.length
  results = []
  for i in [0...length]
    semiResult = []
    for arr in arguments
      semiResult.push arr[i]
    results.push semiResult
  return results

zip([0, 1, 2, 3], [0, -1, -2, -3])
# => [[0, 0], [1, -1], [2, -2], [3, -3]]
```

