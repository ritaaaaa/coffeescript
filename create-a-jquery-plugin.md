# 创建 jQuery 插件

## 问题

你想用 CoffeeScript 来创建 jQuery 插件。

## 方案

```
# 参考 jQuery
$ = jQuery

# 给 jQuery 添加插件对象
$.fn.extend
  # 把 pluginName 改成你的插件名字。
  pluginName: (options) ->
    # 默认设置
    settings =
      option1: true
      option2: false
      debug: false

    # 合并选项与默认设置。
    settings = $.extend settings, options

    # Simple logger.
    log = (msg) ->
      console?.log msg if settings.debug

    # _Insert magic here._
    return @each ()->
      log "Preparing magic show."
      # 你可以使用你的设置了。
      log "Option 1 value: #{settings.option1}"
```

## 讨论

这里有几个关于如何使用新插件的例子。

## JavaScript

```
$("body").pluginName({
  debug: true
});
```

## CoffeeScript

```
$("body").pluginName
  debug: true
```

