# 对象数组
## 问题
你想要得到一个与你的某些属性匹配的数组对象。  

你有一系列的对象，如：
```
cats = [
  {
    name: "Bubbles"
    favoriteFood: "mice"
    age: 1
  },
  {
    name: "Sparkle"
    favoriteFood: "tuna"
  },
  {
    name: "flyingCat"
    favoriteFood: "mice"
    age: 1
  }
]
```
你想用某些特征来滤出想要的对象。例如：猫的位置 ({ 年龄: 1 }) 或者猫的位置 ({ 年龄: 1 , 最爱的食物: "老鼠" })
## 解决方案
你可以像这样来扩展数组：
```
Array::where = (query) ->
    return [] if typeof query isnt "object"
    hit = Object.keys(query).length
    @filter (item) ->
        match = 0
        for key, val of query
            match += 1 if item[key] is val
        if match is hit then true else false

cats.where age:1
# => [ { name: 'Bubbles', favoriteFood: 'mice', age: 1 },{ name: 'flyingCat', favoriteFood: 'mice', age: 1 } ]

cats.where age:1, name: "Bubbles"
# => [ { name: 'Bubbles', favoriteFood: 'mice', age: 1 } ]

cats.where age:1, favoriteFood:"tuna"
# => []
```
## 讨论
这是一个确定的匹配。我们能够让匹配函数更加灵活：
```
Array::where = (query, matcher = (a,b) -> a is b) ->
    return [] if typeof query isnt "object"
    hit = Object.keys(query).length
    @filter (item) ->
        match = 0
        for key, val of query
            match += 1 if matcher(item[key], val)
        if match is hit then true else false

cats.where name:"bubbles"
# => []
# it's case sensitive

cats.where name:"bubbles", (a, b) -> "#{ a }".toLowerCase() is "#{ b }".toLowerCase()
# => [ { name: 'Bubbles', favoriteFood: 'mice', age: 1 } ]
# now it's case insensitive
```
处理收集的一种方式可以被叫做 “find” ，但是像 underscore 或者 lodash 这些库把它叫做 “where” 。

