# 修饰模式

## 问题

你有一组数据，需要在多个过程、可能变换的方式下处理。

## 方案

使用修饰模式来构造如何更改应用。
```
miniMarkdown = (line) ->
    if match = line.match /^(#+)\s*(.*)$/
        headerLevel = match[1].length
        headerText = match[2]
        "<h#{headerLevel}>#{headerText}</h#{headerLevel}>"
    else
        if line.length > 0
            "<p>#{line}</p>"
        else
            ''

stripComments = (line) ->
    line.replace /\s*\/\/.*$/, '' # Removes one-line, double-slash C-style comments

class TextProcessor
    constructor: (@processors) ->

    reducer: (existing, processor) ->
        if processor
            processor(existing or '')
        else
            existing
    processLine: (text) ->
        @processors.reduce @reducer, text
    processString: (text) ->
        (@processLine(line) for line in text.split("\n")).join("\n")

exampleText = '''
              # A level 1 header
              A regular line
              // a comment
              ## A level 2 header
              A line // with a comment
              '''

processor = new TextProcessor [stripComments, miniMarkdown]

processor.processString exampleText

# => "<h1>A level 1 header</h1>\n<p>A regular line</p>\n\n<h2>A level 2 header</h2>\n<p>A line</p>"
```

## 结果
```
<h1>A level 1 header</h1>
<p>A regular line</p>

<h2>A level 1 header</h2>
<p>A line</p>
```

## 讨论

TextProcessor 服务有修饰的作用，可将个人、专业文本处理器绑定在一起。这使 miniMarkdown 和 stripComments 组件只专注于处理一行文本。未来的开发人员只需要编写函数返回一个字符串，并将它添加到阵列的处理器。

我们甚至可以修改现有的修饰对象动态:
```
smilies =
    ':)' : "smile"
    ':D' : "huge_grin"
    ':(' : "frown"
    ';)' : "wink"

smilieExpander = (line) ->
    if line
        (line = line.replace symbol, "<img src='#{text}.png' alt='#{text}' />") for symbol, text of smilies
    line

processor.processors.unshift smilieExpander

processor.processString "# A header that makes you :) // you may even laugh"

# => "<h1>A header that makes you <img src='smile.png' alt='smile' /></h1>"

processor.processors.shift()

# => "<h1>A header that makes you :)</h1>"
```
















