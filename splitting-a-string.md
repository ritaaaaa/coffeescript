# 拆分字符串

## 问题

你想拆分一个字符串。

## 解决方案

使用 JavaScript 字符串的 split() 方法：

```

	"foo bar baz".split " "
	# => [ 'foo', 'bar', 'baz' ]

```

## 讨论

String 的这个 split() 方法是标准的 JavaScript 方法。可以用来基于任何分隔符——包括正则表达式来拆分字符串。这个方法还可以接受第二个参数，用于指定返回的子字符串数目。

```

	"foo-bar-baz".split "-"
	# => [ 'foo', 'bar', 'baz' ]

```

```
	
	"foo   bar  \t baz".split /\s+/
	# => [ 'foo', 'bar', 'baz' ]

```

```
	
	"the sun goes down and I sit on the old broken-down river pier".split " ", 2
	# => [ 'the', 'sun' ]

```

