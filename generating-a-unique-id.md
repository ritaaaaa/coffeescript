# 生成唯一ID

## 问题

你想随机生成一个唯一的标识符。

## 解决方案

可以根据一个随机数值生成一个 Base 36 编码的字符串。

```
uniqueId = (length=8) ->
  id = ""
  id += Math.random().toString(36).substr(2) while id.length < length
  id.substr 0, length

uniqueId()    # => n5yjla3b
uniqueId(2)   # => 0d
uniqueId(20)  # => ox9eo7rt3ej0pb9kqlke
uniqueId(40)  # => xu2vo4xjn4g0t3xr74zmndshrqlivn291d584alj
```
	
## 讨论

使用其他技术也可以，但这种方法相对来说性能更高，也更灵活。
