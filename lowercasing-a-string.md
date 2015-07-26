# 把字符串转换为小写形式

## 问题

你想把字符串转换成小写形式。

## 解决方案

使用 JavaScript 的 String 的 toLowerCase() 方法：

```

	"ONE TWO THREE".toLowerCase()
	# => 'one two three'

```

## 讨论

toLowerCase() 是一个标准的 JavaScript 方法。不要忘了带圆括号。

## 语法糖

通过下面的快捷方式可以添加某种类似　Ruby 的语法糖：

```

	String::downcase = -> @toLowerCase()
	"ONE TWO THREE".downcase()
	# => 'one two three'

```

上面的代码演示了 CoffeeScript 的两个特性:

* 双冒号 :: 是引用 .prototype 的快捷方式；
* “at” 字符 @ 是引用 this 的快捷方式。

上面的代码会编译成如下 JavaScript 代码：

```

	String.prototype.downcase = function() {
	  return this.toLowerCase();
	};
	"ONE TWO THREE".downcase();

```

**提示**
尽管上面的用法在类似 Ruby 的语言中很常见，在 JavaScript 中对本地对象的扩展经常被视为不好的。（请看：[Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/);[Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)）
