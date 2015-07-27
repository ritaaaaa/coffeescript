# 映射数组
## 问题
你有一个对象数组，想把这些对象映射到另一个数组中，就像 Ruby 的映射一样。
## 解决方案
使用 map() 和匿名函数，但不要忘了还有列表推导。
```
electric_mayhem = [ { name: "Doctor Teeth", instrument: "piano" },
                    { name: "Janice", instrument: "lead guitar" },
                    { name: "Sgt. Floyd Pepper", instrument: "bass" },
                    { name: "Zoot", instrument: "sax" },
                    { name: "Lips", instrument: "trumpet" },
                    { name: "Animal", instrument: "drums" } ]

names = electric_mayhem.map (muppet) -> muppet.name
# => [ 'Doctor Teeth', 'Janice', 'Sgt. Floyd Pepper', 'Zoot', 'Lips', 'Animal' ]
```
## 讨论
因为 CoffeeScript 支持匿名函数，所以在 CoffeeScript 中映射数组就像在 Ruby 中一样简单。
映射在 CoffeeScript 中是处理复杂转换和连缀映射的好方法。如果你的转换如同上例中那么简单，那可能将它当成[列表推导]( http://coffeescript-cookbook.github.io/chapters/arrays/list-comprehensions) 看起来会清楚一些。

