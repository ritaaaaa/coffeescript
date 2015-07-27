# AJAX

## 问题

你想要使用 jQuery 来调用 AJAX。

## 方案

```
$ ?= require 'jquery' # 由于 Node.js 的兼容性

$(document).ready ->
    # 基本示例
    $.get '/', (data) ->
        $('body').append "Successfully got the page."

    $.post '/',
        userName: 'John Doe'
        favoriteFlavor: 'Mint'
        (data) -> $('body').append "Successfully posted to the page."

    # 高级设置
    $.ajax '/',
        type: 'GET'
        dataType: 'html'
        error: (jqXHR, textStatus, errorThrown) ->
            $('body').append "AJAX Error: #{textStatus}"
        success: (data, textStatus, jqXHR) ->
            $('body').append "Successful AJAX call: #{data}"
```

jQuery 1.5 和更新版本都增加了一种新的补充的 API，用于处理不同的回调。

```
request = $.get '/'
    request.success (data) -> $('body').append "Successfully got the page again."
    request.error (jqXHR, textStatus, errorThrown) -> $('body').append "AJAX Error: ${textStatus}."
```

## 讨论

其中的 jQuery 和 $ 变量可以互换使用。另请参阅[回调绑定](http://coffeescript-cookbook.github.io/chapters/jquery/callback-bindings-jquery)。

