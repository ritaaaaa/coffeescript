## 列表推导
### 问题
你有一个对象数组，想将它们映射到另一个数组，类似于 Python 的列表推导。
### 解决方案
使用列表推导，不要忘记[mapping-arrays]( http://coffeescript-cookbook.github.io/chapters/arrays/mapping-arrays) 。
```
electric_mayhem = [ { name: "Doctor Teeth", instrument: "piano" },
                    { name: "Janice", instrument: "lead guitar" },
                    { name: "Sgt. Floyd Pepper", instrument: "bass" },
                    { name: "Zoot", instrument: "sax" },
                    { name: "Lips", instrument: "trumpet" },
                    { name: "Animal", instrument: "drums" } ]

names = (muppet.name for muppet in electric_mayhem)
# => [ 'Doctor Teeth', 'Janice', 'Sgt. Floyd Pepper', 'Zoot', 'Lips', 'Animal' ]
```
### 讨论
因为 CoffeeScript 直接支持列表推导，在你使用一个 Python 的语句时，他们会很好地起到作用。对于简单的映射，列表推导具有更好的可读性。但是对于复杂的转换或链式映射，映射数组可能更合适。

