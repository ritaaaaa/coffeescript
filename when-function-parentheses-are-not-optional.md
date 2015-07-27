# 当函数括号不可选

## 问题

你想要调用一个没有参数的函数，但不希望使用括号。

## 方案

不管怎样都使用括号。

另一个方法是使用 do 表示法，如下：

```
notify = -> alert "Hello, user!"
do notify if condition
```

编译成 JavaScript 则可表示为：

```
var notify;
notify = function() {
    return alert("Hello, user!");
};
if (condition) {
    notify();
}
```

## 讨论

这个方法与 Ruby 类似，在于都可以不使用括号来完成方法的调用。而不同点在于，CoffeeScript 把空的函数名作为函数的指针。这样以来，如果你不赋予一个方法任何参数，那么 CoffeeScript 将无法分辨你是想要调用函数还是把它作为引用。

这是好是坏呢？其实只是有所不同。它创造了一个意想不到的语法实例——括号并不*总是*可选的——但是它能让你流利地使用名字来传递和接收函数，这对于 Ruby 来说是难以实现的。

对于 CoffeeScript 来说，使用 do 表示法是一个巧妙的方法来克服括号使用恐惧症。尽管有部分人宁愿在函数调用中写出所有括号。

