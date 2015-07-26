# 将数组连接

## 问题

你希望将两个数组连接到一起。

## 解决方案

在 JavaScript 中，有两个标准方法可以用来连接数组。

第一种是使用 JavaScript 的数组方法 concat()：

```

	array1 = [1, 2, 3]
	array2 = [4, 5, 6]
	array3 = array1.concat array2
	# => [1, 2, 3, 4, 5, 6]

```

需要指出的是 array1 没有被运算修改。连接后形成的新数组的返回值是一个新的对象。

如果你希望在连接两个数组后不产生新的对象，那么你可以使用下面的技术：

```

	array1 = [1, 2, 3]
	array2 = [4, 5, 6]
	Array::push.apply array1, array2
	array1
	# => [1, 2, 3, 4, 5, 6]

```

在上面的例子中，Array.prototype.push.apply(a, b) 方法修改了 array1 而没有产生一个新的数组对象。

在 CoffeeScript 中，我们可以简化上面的方式，通过给数组创建一个新方法 merge()：

```

	Array::merge = (other) -> Array::push.apply @, other

	array1 = [1, 2, 3]
	array2 = [4, 5, 6]
	array1.merge array2
	array1
	# => [1, 2, 3, 4, 5, 6]

```

另一种方法，我可以直接将一个 CoffeeScript splat(array2) 放入 push() 中，避免了使用数组原型。

```

	array1 = [1, 2, 3]
	array2 = [4, 5, 6]
	array1.push array2...
	array1
	# => [1, 2, 3, 4, 5, 6]

```

一个更加符合语言习惯的方法是在一个数组语言中直接使用 splat 运算符(...)。这可以用来连接任意数量的数组。

```

	array1 = [1, 2, 3]
	array2 = [4, 5, 6]
	array3 = [array1..., array2...]
	array3
	# => [1, 2, 3, 4, 5, 6]

```

## 讨论

CoffeeScript 缺少了一种用来连接数组的特殊语法，但是 concat() 和 push() 是标准的 JavaScript 方法。
