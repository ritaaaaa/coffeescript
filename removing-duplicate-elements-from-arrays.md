## 删除数组中的相同元素
### 问题
你想从数组中删除相同元素。
### 解决方案
```
Array::unique = ->
  output = {}
  output[@[key]] = @[key] for key in [0...@length]
  value for key, value of output

[1,1,2,2,2,3,4,5,6,6,6,"a","a","b","d","b","c"].unique()
# => [ 1, 2, 3, 4, 5, 6, 'a', 'b', 'd', 'c' ]
```
### 讨论
在 JavaScript 中有很多的独特方法来实现这一功能。这一次是基于“最快速的方法来查找数组的唯一元素”，出自[这里](http://www.shamasis.net/2009/09/fast-algorithm-to-find-unique-items-in-javascript-array/) 。
>注意: 延长本机对象通常被认为是在 JavaScript 不好的做法，即便它在 Ruby 语言中相当普遍， (参考:[Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/)  


