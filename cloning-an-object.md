# 克隆对象（深复制）

## 问题

你想复制一个对象，包含其所有子对象。

## 解决方案

```

	clone = (obj) ->
	  if not obj? or typeof obj isnt 'object'
	    return obj
	
	  newInstance = new obj.constructor()
	
	  for key of obj
	    newInstance[key] = clone obj[key]
	
	  return newInstance
	
	x =
	  foo: 'bar'
	  bar: 'foo'
	
	y = clone(x)
	
	y.foo = 'test'
	
	console.log x.foo isnt y.foo, x.foo, y.foo
	# => true, bar, test
	
```

## 讨论

通过赋值来复制对象与通过克隆函数来复制对象的区别在于如何处理引用。赋值只会复制对象的引用，而克隆函数则会:

* 创建一个全新的对象
* 这个新对象会复制原对象的所有属性，
* 并且对原对象的所有子对象，也会递归调用克隆函数，复制每个子对象的所有属性。

下面是一个通过赋值来复制对象的例子：

```

	x =
	  foo: 'bar'
	  bar: 'foo'
	
	y = x
	
	y.foo = 'test'
	
	console.log x.foo isnt y.foo, x.foo, y.foo
	# => false, test, test
	
```

显然，复制之后修改 y 也就修改了 x。
