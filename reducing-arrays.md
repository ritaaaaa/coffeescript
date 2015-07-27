# 归纳数组

## 问题
你有一个对象数组，想要把它们归纳为一个值，类似于 Ruby 中的 reduce() 和 reduceRight() 。
## 解决方案
可以使用一个匿名函数包含 Array 的 reduce() 和 reduceRight() 方法，保持代码清晰易懂。这里归纳可能会像对数值和字符串应用 + 运算符那么简单。
```
[1,2,3,4].reduce (x,y) -> x + y
# => 10

["words", "of", "bunch", "A"].reduceRight (x, y) -> x + " " + y
# => 'A bunch of words'
```
或者，也可能更复杂一些，例如把列表中的元素聚集到一个组合对象中。
```
people =
    { name: 'alec', age: 10 }
    { name: 'bert', age: 16 }
    { name: 'chad', age: 17 }

people.reduce (x, y) ->
    x[y.name]= y.age
    x
, {}
# => { alec: 10, bert: 16, chad: 17 }
```
### 讨论
Javascript 1.8 中引入了 reduce 和 reduceRight ，而 Coffeescript 为匿名函数提供了简单自然的表达语法。二者配合使用，可以把集合的项合并为组合的结果。


