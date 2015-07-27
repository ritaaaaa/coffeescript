# 嵌入 JavaScript

## 问题

你想在 CoffeeScript 中嵌入找到的或预先编写的 JavaScript 代码。

## 解决方案

把 JavaScript 包装到撇号中：

```

	`function greet(name) {
	    return "Hello "+name;
	}`

	# 回到CoffeeScript中
	greet "Coffee"
	# => "Hello Coffee"

```

## 讨论

这是在 CoffeeScript 代码中集成少量 JavaScript 而不必用 CoffeeScript 语法转换它们的最简单的方法。正如 [CoffeeScript Language Reference](http://jashkenas.github.com/coffee-script/#embedded) 中展示的，可以在一定范围内混合这两种语言的代码：

```

	hello = `function (name) {
	    return "Hello "+name
	}`
	hello "Coffee"
	# => "Hello Coffee"

```

这里的变量 "hello" 还在 CoffeeScript 中，但赋给它的函数则是用 JavaScript 写的。
