# 创建一个不存在的对象字面值

## 问题

你想初始化一个对象字面值，但如果这个对象已经存在，你不想重写它。

## 解决方案

使用存在判断运算符（existential operator）。

```

	window.MY_NAMESPACE ?= {}

```

## 讨论

这行代码与下面的 JavaScript 代码等价：

```

	window.MY_NAMESPACE = window.MY_NAMESPACE || {};

```

这是 JavaScript 中一个常用的技巧，即使用对象字面值来定义命名空间。这样先判断是否存在同名的命名空间然后再创建，可以避免重写已经存在的命名空间。
