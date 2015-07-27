# 命令模式

## 问题

你需要让另一个对象处理你自己的可执行的代码。

## 方案

使用 [Command pattern](http://en.wikipedia.org/wiki/Command_pattern) 传递函数的引用。
```
# Using a private variable to simulate external scripts or modules
incrementers = (() ->
    privateVar = 0

    singleIncrementer = () ->
        privateVar += 1

    doubleIncrementer = () ->
        privateVar += 2
    
    commands = 
        single: singleIncrementer
        double: doubleIncrementer
        value: -> privateVar
)()

class RunsAll
    constructor: (@commands...) ->
    run: -> command() for command in @commands

runner = new RunsAll(incrementers.single, incrementers.double, incrementers.single, incrementers.double)
runner.run()
incrementers.value() # => 6
```

## 讨论

以函数作为一级的对象且从 Javascript 函数的变量范围中继承，CoffeeScript 语言模式几乎看不见。事实上，任何函数传递回调函数可以作为一个*命令*。

jqXHR 对象返回 jQuery AJAX 方法使用此模式。
```
jqxhr = $.ajax
    url: "/"

logMessages = ""

jqxhr.success -> logMessages += "Success!\n"
jqxhr.error -> logMessages += "Error!\n"
jqxhr.complete -> logMessages += "Completed!\n"

# On a valid AJAX request:
# logMessages == "Success!\nCompleted!\n"
```














