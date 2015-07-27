# 字符串插值

## 问题

你想创建一个字符串，让它包含体现某个 CoffeeScript 变量的文本。

## 解决方案

使用 CoffeeScript 中类似 Ruby 的字符串插值，而不是 JavaScript 的字符串拼接。

插值：

```

muppet = "Beeker"
favorite = "My favorite muppet is #{muppet}!"

# => "My favorite muppet is Beeker!"
```

```
square = (x) -> x * x
message = "The square of 7 is #{square 7}."

# => "The square of 7 is 49."
```

## 讨论

CoffeeScript 的插值与 Ruby 类似，多数表达式都可以用在 #{ ... } 插值结构中。

CoffeeScript 支持在插值结构中放入多个有副作用的表达式，但建议大家不要这样做。因为只有表达式的最后一个值会被插入。

```
# 可以这样做，但不要这样做。否则，你会疯掉。
square = (x) -> x * x
muppet = "Beeker"
message = "The square of 10 is #{muppet='Animal'; square 10}. Oh, and your favorite muppet is now #{muppet}."

# => "The square of 10 is 100. Oh, and your favorite muppet is now Animal."
```

