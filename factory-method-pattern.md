# 工厂方法模式

## 问题

直到开始运行你才知道需要的是什么种类的对象。

## 方案

使用 [Factory Method](http://en.wikipedia.org/wiki/Factory_method_pattern) 模式和选择对象都是动态生成的。

你需要将一个文件加载到编辑器，但是直到用户选择文件时你才知道它的格式。一个类使用工厂方法模式可以根据文件的扩展名提供不同的解析器。
```
class HTMLParser
    constructor: ->
        @type = "HTML parser"
class MarkdownParser
    constructor: ->
        @type = "Markdown parser"
class JSONParser
    constructor: ->
        @type = "JSON parser"

class ParserFactory
    makeParser: (filename) ->
        matches = filename.match /\.(\w*)$/
        extension = matches[1]
        switch extension
            when "html" then new HTMLParser
            when "htm" then new HTMLParser
            when "markdown" then new MarkdownParser
            when "md" then new MarkdownParser
            when "json" then new JSONParser

factory = new ParserFactory

factory.makeParser("example.html").type # => "HTML parser"

factory.makeParser("example.md").type # => "Markdown parser"

factory.makeParser("example.json").type # => "JSON parser"
```

## 讨论

在这个示例中，你可以关注解析的内容，忽略细节文件的格式。更先进的工厂方法，例如，搜索版本控制文件中的数据本身，然后返回一个更精确的解析器(例如，返回一个 HTML5 解析器而不是 HTML v4 解析器)。












