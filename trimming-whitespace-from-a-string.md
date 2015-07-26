# 清理字符串前后的空白符

## 问题

你想清理字符串前后的空白符。

## 解决方案

使用 JavaScript 的正则表达式来替换空白符。

要清理字符串前后的空白符，可以使用以下代码：

```

	"  padded string  ".replace /^\s+|\s+$/g, ""
	# => 'padded string'

```

如果只想清理字符串前面的空白符，使用以下代码：

```

	"  padded string  ".replace /^\s+/g, ""
	# => 'padded string  '

```

如果只想清理字符串后面的空白符，使用以下代码：

```

	"  padded string  ".replace /\s+$/g, ""
	# => '  padded string'

```

## 讨论

Opera、Firefox 和 Chrome 中 String 的原型都有原生的 trim 方法，其他浏览器也可以添加一个。对于这个方法而言，还是尽可能使用内置方法，否则就创建一个 polyfill：

```

	unless String::trim then String::trim = -> @replace /^\s+|\s+$/g, ""
	
	"  padded string  ".trim()
	# => 'padded string'

```

## 语法糖

还可以添加一些类似Ruby中的语法糖，定义如下快捷方法：

```

	String::strip = -> if String::trim? then @trim() else @replace /^\s+|\s+$/g, ""
	String::lstrip = -> @replace /^\s+/g, ""
	String::rstrip = -> @replace /\s+$/g, ""
	
	"  padded string  ".strip()
	# => 'padded string'
	"  padded string  ".lstrip()
	# => 'padded string  '
	"  padded string  ".rstrip()
	# => '  padded string'

```

要想深入了解 JavaScript 执行 trim 操作时的性能，请参见 Steve Levithan 的[这篇博客文章](http://blog.stevenlevithan.com/archives/faster-trim-javascript)。
