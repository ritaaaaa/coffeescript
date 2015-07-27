# 基本的 HTTP 客户端

## 问题

你想创建一个 HTTP 客户端。

## 方案

在这个方法中,我们将使用 [node.js's](http://nodejs.org/) HTTP 库。我们将从一个简单的客户端 GET 请求示例返回计算机的外部 IP。

## 关于 GET
```
http = require 'http'

http.get { host: 'www.google.com' }, (res) ->
    console.log res.statusCode
```

get 函数，从 node.js's http 模块，发出一个 GET 请求到一个 http 服务器。响应是以回调的形式，我们可以在一个函数中处理。这个例子仅仅输出响应状态码。检查一下：
```
$ coffee http-client.coffee 
200
```

## 我的 IP 是什么?

如果你是在一个类似局域网的依赖于 NAT 的网络中,你可能会面临找出外部 IP 地址的问题。让我们为这个问题写一个小的 coffeescript。
```
http = require 'http'

http.get { host: 'checkip.dyndns.org' }, (res) ->
    data = ''
    res.on 'data', (chunk) ->
        data += chunk.toString()
    res.on 'end', () ->
        console.log data.match(/([0-9]+\.){3}[0-9]+/)[0]
```

我们可以从监听 'data' 事件的结果对象中得到数据，知道它结束了一次 'end' 的触发事件。当这种情况发生时，我们可以做一个简单的正则表达式来匹配我们提取的 IP 地址。试一试：
```
$ coffee http-client.coffee 
123.123.123.123
```

## 讨论

请注意 http.get 是 http.request 的快捷方式。后者允许您使用不同的方法发出 HTTP 请求，如 POST 或 PUT。

在这个问题上的 API 和整体信息，检查 node.js's [http](http://nodejs.org/docs/latest/api/http.html) 和 [https](http://nodejs.org/docs/latest/api/https.html) 文档页面。此外, [HTTP spec](http://www.ietf.org/rfc/rfc2616.txt) 可能派上用场。


## 练习


- 为键值存储 HTTP 服务器创建一个客户端，使用基本的 HTTP 服务器方法。











