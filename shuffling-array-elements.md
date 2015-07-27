# 打乱数组中的元素
## 问题
你想打乱数组中的元素。
## 解决方案
 [ Fisher-Yates shuffle ](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) 是一种高效、公正的方式来让数组中的元素随机化。这是一个相当简单的方法：在列表的结尾处开始，用一个随机元素交换最后一个元素列表中的最后一个元素。继续下一个并重复操作，直到你到达列表的起始端，最终列表中所有的元素都已打乱。这 [ Fisher-Yates shuffle Visualization ]( http://bost.ocks.org/mike/shuffle/) 可以帮助你理解算法。
```
shuffle = (source) ->
  # Arrays with < 2 elements do not shuffle well. Instead make it a noop.
  return source unless source.length >= 2
  # From the end of the list to the beginning, pick element `index`.
  for index in [source.length-1..1]
    # Choose random element `randomIndex` to the front of `index` to swap with.
    randomIndex = Math.floor Math.random() * (index + 1)
    # Swap `randomIndex` with `index`, using destructured assignment
    [source[index], source[randomIndex]] = [source[randomIndex], source[index]]
  source

shuffle([1..9])
# => [ 3, 1, 5, 6, 4, 8, 2, 9, 7 ]
```
## 讨论
###  一种错误的方式
有一个很常见但是错误的打乱数组的方式：通过随机数。
```
shuffle = (a) -> a.sort -> 0.5 - Math.random()

```
如果你做了一个随机的排序，你应该得到一个序列随机的顺序，对吧？甚至[微软也用这种随机排序算法](http://www.robweir.com/blog/2010/02/microsoft-random-browser-ballot.html) 。原来，[这种随机排序算法产生有偏差的结果]( http://blog.codinghorror.com/the-danger-of-naivete/) ，因为它存在一种洗牌的错觉。随机排序不会导致一个工整的洗牌，它会导致序列排序质量的参差不齐。
### 速度和空间的优化
以上的解决方案处理速度是不一样的。该列表，当转换成 JavaScript 时，比它要复杂得多，变性分配比处理裸变量的速度要慢得多。以下代码并不完善，并且需要更多的源代码空间 … 但会编译量更小，运行更快：
```
shuffle = (a) ->
  i = a.length
  while --i > 0
    j = ~~(Math.random() * (i + 1)) # ~~ is a common optimization for Math.floor
    t = a[j]
    a[j] = a[i]
    a[i] = t
  a
```
### 扩展 Javascript 来包含乱序数组
下面的代码将乱序功能添加到数组原型中，这意味着你可以在任何希望的数组中运行它，并以更直接的方式来运行它。
```
Array::shuffle ?= ->
  if @length > 1 then for i in [@length-1..1]
    j = Math.floor Math.random() * (i + 1)
    [@[i], @[j]] = [@[j], @[i]]
  this

[1..9].shuffle()
# => [ 3, 1, 5, 6, 4, 8, 2, 9, 7 ]
```
>注意: 虽然它像在 Ruby 语言中相当普遍，但是在 JavaScript 中扩展本地对象通常被认为是不太好的做法 ( 参考: [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/)    
正如提到的，以上的代码的添加是十分安全的。它仅仅需要添 Array :: shuffle 如果它不存在，就要添加赋值运算符 (?=) 。这样，我们就不会重写到别人的代码，或是本地浏览器的方式。  

同时，如果你认为你会使用很多的实用功能，可以考虑使用一个工具库，像 [ Lo-dash](https://lodash.com/) 。他们有很多功能，像跨浏览器的简洁高效的地图。 [Underscore](http://underscorejs.org/) 也是一个不错的选择。


