# 大写单词首字母

## 问题

你想把字符串中每个单词的首字母转换为大写形式。

## 解决方案

使用“拆分-映射-拼接”模式：先把字符串拆分成单词，然后通过映射来大写单词第一个字母小写其他字母，最后再将转换后的单词拼接成字符串。

```

	("foo bar baz".split(' ').map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '
	# => 'Foo Bar Baz'

```

或者使用列表推导（comprehension），也可以实现同样的结果：

```

	(word[0].toUpperCase() + word[1..-1].toLowerCase() for word in "foo   bar   baz".split /\s+/).join ' '
	# => 'Foo Bar Baz'

```

## 讨论

“拆分-映射-拼接”是一种常用的脚本编写模式，可以追溯到Perl语言。如果能把这个功能直接通过“[扩展类](http://coffeescript-cookbook.github.io/chapters/objects/extending-classes)”放到String类里，就更方便了。

需要注意的是，“拆分-映射-拼接”模式存在两个问题。第一个问题，只有在文本形式统一的情况下才能有效拆分文本。如果来源字符串中有分隔符包含多个空白符，就需要考虑怎么过滤掉多余的空单词。一种解决方案是使用正则表达式来匹配空白符的串，而不是像前面那样只匹配一个空格：

```

	("foo    bar    baz".split(/\s+/).map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '
	# => 'Foo Bar Baz'

```

但这样做又会导致第二个问题：在结果字符串中，原来的空白符串经过拼接就只剩下一个空格了。

不过，一般来说，这两个问题还是可以接受的。所以，“拆分-映射-拼接”仍然是一种有效的技术。
