# 扩展内置对象

## 问题

你想要扩展一个类来增加新的函数或者替换旧的。

## 方案

使用 :: 把你的新函数分配到对象或者类的原型中。

```
String::capitalize = () ->
  (this.split(/\s+/).map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '

"foo bar     baz".capitalize()
# => 'Foo Bar Baz'
```

## 讨论

在 JavaScript （同样地，在 CoffeeScript ）中，对象都有一个原型成员，它定义了什么成员函数能够适用于基于该原型的所有对象。在CoffeeScript中，你可以使用 :: 捷径来直接访问这个原型。

**注意：**虽然这种做法在很多种语言中相当普遍，比如 Ruby，但是在 JavaScript 中，扩展本地对象通常被认为是不那么好的做法（可参考：[可维护的 JavaScript：不要修改你不拥有的对象](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/)；[扩展内置的本地对象。邪恶与否？](http://perfectionkills.com/extending-native-builtins/)。）


