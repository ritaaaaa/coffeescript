# CoffeeScrip 的 type 函数

## 问题

你想在不使用 typeof 的情况下知道一个函数的类型。（要了解为什么 typeof 不靠谱，请参见 [http://javascript.crockford.com/remedial.html](http://javascript.crockford.com/remedial.html)。）

## 解决方案

使用下面这个type函数

```

	type = (obj) ->
    if obj == undefined or obj == null
      return String obj
    classToType = {
      '[object Boolean]': 'boolean',
      '[object Number]': 'number',
      '[object String]': 'string',
      '[object Function]': 'function',
      '[object Array]': 'array',
      '[object Date]': 'date',
      '[object RegExp]': 'regexp',
      '[object Object]': 'object'
    }
    return classToType[Object.prototype.toString.call(obj)]
```

## 讨论

这个函数模仿了 jQuery 的 [$.type函数](http://api.jquery.com/jQuery.type/)。

需要注意的是，在某些情况下，只要使用鸭子类型检测及存在运算符就可以不必检测对象的类型了。例如，下面这行代码不会发生异常，它会在 myArray 的确是数组（或者一个带有 push 方法的类数组对象）的情况下向其中推入一个元素，否则什么也不做。

```

	myArray?.push? myValue

```

