# 比较范围

## 问题

如果你想知道某个变量是否在给定的范围内。

## 解决方案

使用 CoffeeScript 的连缀比较语法。

```
	
	maxDwarfism = 147
	minAcromegaly = 213
	
	height = 180
	normalHeight = maxDwarfism < height < minAcromegaly
	# => true

```

## 讨论

这是从 Python 中借鉴过来的一个很棒的特性。利用这个特性，不必像下面这样写出完整的比较：

```

	normalHeight = height > maxDwarfism && height < minAcromegaly

```

CoffeeScript 支持像写数学中的比较表达式一样连缀两个比较，这样更直观。
