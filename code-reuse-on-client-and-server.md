# 服务端和客户端的代码重用

## 问题

当你在 CoffeeScript 上创建了一个函数，并希望将它用在有网页浏览器的客户端和有 Node.js 的服务端时。

## 解决方案

以下列方法输出函数：

```
# simpleMath.coffee

# these methods are private
add = (a, b) ->
    a + b

subtract = (a, b) ->
    a - b

square = (x) ->
    x * x

# create a namespace to export our public methods
SimpleMath = exports? and exports or @SimpleMath = {}

# items attached to our namespace are available in Node.js as well as client browsers
class SimpleMath.Calculator
    add: add
    subtract: subtract
    square: square
```


## 讨论

在上面的例子中，我们创建了一个新的名为 “SimpleMath” 的命名空间。如果 “export” 是有效的，我们的类就会作为一个 Node.js 模块输出。如果 “export” 是无效的，那么 “SimpleMath” 就会被加入全局命名空间，这样就可以被我们的网页使用了。

在 Node.js 中，我们可以使用 “require” 命令包含我们的模块。

```
$ node

> var SimpleMath = require('./simpleMath');
undefined
> var Calc = new SimpleMath.Calculator();
undefined
> console.log("5 + 6 = ", Calc.add(5, 6));
5 + 6 =  11
undefined
>
```

在网页中，我们可以通过将模块作为一个脚本嵌入其中。

```
<!DOCTYPE HTML>
<html lang="en-US">
<head>
    <meta charset="UTF-8">
    <title>SimpleMath Module Example</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script src="simpleMath.js"></script>
    <script>
        jQuery(document).ready(function    (){
            var Calculator = new SimpleMath.Calculator();
            var result = $('<li>').html("5 + 6 = " + Calculator.add(5, 6));
            $('#SampleResults').append(result); 
        });
    </script>
</head>
<body>
    <h1>A SimpleMath Example</h1>
    <ul id="SampleResults"></ul>
</body>
</html>
```

输出结果：

**A SimpleMath Example**

**· 5 + 6 = 11**
