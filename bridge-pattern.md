# 桥接模式

## 问题

你需要保持一个可靠的接口代码，可以经常变化或者在多种实现间转换。

## 方案

使用桥接模式作为不同实现和其余代码的中间体。

假设您开发了一个浏览器的文本编辑器保存到云。然而，现在你需要通过独立客户端的端口将其在本地保存。
```
class TextSaver
    constructor: (@filename, @options) ->
    save: (data) ->

class CloudSaver extends TextSaver
    constructor: (@filename, @options) ->
        super @filename, @options
    save: (data) ->
        # Assuming jQuery
        # Note the fat arrows
        $( =>
            $.post "#{@options.url}/#{@filename}", data, =>
                alert "Saved '#{data}' to #{@filename} at #{@options.url}."
        )

class FileSaver extends TextSaver
    constructor: (@filename, @options) ->
        super @filename, @options
        @fs = require 'fs'
    save: (data) ->
        @fs.writeFile @filename, data, (err) => # Note the fat arrow
            if err? then console.log err
            else console.log "Saved '#{data}' to #{@filename} in #{@options.directory}."

filename = "temp.txt"
data = "Example data"

saver = if window?
    new CloudSaver filename, url: 'http://localhost' # => Saved "Example data" to temp.txt at http://localhost
else if root?
    new FileSaver filename, directory: './' # => Saved "Example data" to temp.txt in ./

saver.save data
```

## 讨论

桥接模式可以帮助你将特定实现的代码置于看不见的地方，这样你就可以专注于你的程序中的具体代码。在上面的示例中,应用程序的其余部分可以称为 saver.save data ，不考虑文件的最终结束。








