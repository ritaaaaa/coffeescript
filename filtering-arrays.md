## 筛选数组
### 问题
你想要根据布尔条件来筛选数组。
### 方案
使用 Array.filter (ECMAScript 5)： array = [1..10]
```
array.filter (x) -> x > 5
# => [6,7,8,9,10]
```
在 EC5 之前的实现中，可以通过添加一个筛选函数扩展 Array 的原型，该函数接受一个回调并对自身进行过滤，将回调函数返回 true 的元素收集起来。
```
# 扩展 Array 的原型
Array::filter = (callback) ->
  element for element in this when callback(element)

array = [1..10]

# 筛选偶数
filtered_array = array.filter (x) -> x % 2 == 0
# => [2,4,6,8,10]

# 过滤掉小于或等于5的元素
gt_five = (x) -> x > 5
filtered_array = array.filter gt_five
# => [6,7,8,9,10]
```
### 讨论
这个方法与 Ruby 的Array 的 #select 方法类似。

