# 检查变量的类型是否为数组

## 问题
你希望检查一个变量是否为一个数组。

```
myArray = []
console.log typeof myArray // outputs 'object'
```

“typeof” 运算符为数组输出了一个错误的结果。

## 解决方案

使用下面的代码：

```
typeIsArray = Array.isArray || ( value ) -> return {}.toString.call( value ) is '[object Array]'
```

为了使用这个，像下面这样调用 typeIsArray 就可以了。

```
myArray = []
typeIsArray myArray // outputs true
```

## 讨论

上面方法取自 "the Miller Device"。另外一个方式是使用 Douglas Crockford 的片段。

```
typeIsArray = ( value ) ->
    value and
        typeof value is 'object' and
        value instanceof Array and
        typeof value.length is 'number' and
        typeof value.splice is 'function' and
        not ( value.propertyIsEnumerable 'length' )
```

