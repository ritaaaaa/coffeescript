# 基本的 HTTP 服务器

## 问题

你想在网络上创建一个 HTTP 服务器。在这个方法中，我们将逐步从最小的服务器成为一个功能键值存储。

## 方案

我们将使用 [node.js](http://nodejs.org/) HTTP 库并在 Coffeescript 中创建最简单的 web 服务器。

## 说 'hi\n'

我们可以通过导入 node.js HTTP 模块开始。这会包含 createServer，一个简单的请求处理程序返回 HTTP 服务器。我们可以使用该服务器监听 TCP 端口。
```
http = require 'http'
server = http.createServer (req, res) -> res.end 'hi\n'
server.listen 8000
```

要运行这个例子，只需放在一个文件中并运行它。你可以用 ctrl-c 终止它。我们可以使用 curl 命令测试它,可用在大多数 *nix 平台:
```
$ curl -D - http://localhost:8000/
HTTP/1.1 200 OK
Connection: keep-alive
Transfer-Encoding: chunked

hi
```

## 发生什么事了?

让我们一点点来反馈服务器上发生的事情。这时，我们可以友好的对待用户并提供他们一些 HTTP 头文件。
```
http = require 'http'

server = http.createServer (req, res) ->
    console.log req.method, req.url
    data = 'hi\n'
    res.writeHead 200,
        'Content-Type':     'text/plain'
        'Content-Length':   data.length
    res.end data

server.listen 8000
```

再次尝试访问它，但是这一次使用不同的 URL 路径，比如 http://localhost:8000/coffee。你会看到这样的服务器控制台:
```
$ coffee http-server.coffee 
GET /
GET /coffee
GET /user/1337
```

## 得到的东西

假如我们的网络服务器能够保存一些数据会怎么样？我们将在通过 GET 请求检索的元素中设法想出一个简单的键值存储。提供一个关键路径，服务器将请求返回相应的值,如果不存在则返回404。
```
http = require 'http'

store = # we'll use a simple object as our store
    foo:    'bar'
    coffee: 'script'

server = http.createServer (req, res) ->
    console.log req.method, req.url
    
    value = store[req.url[1..]]

    if not value
        res.writeHead 404
    else
        res.writeHead 200,
            'Content-Type': 'text/plain'
            'Content-Length': value.length + 1
        res.write value + '\n'
    
    res.end()

server.listen 8000
```

我们可以试试几种 url，看看它们如何回应:
```
$ curl -D - http://localhost:8000/coffee
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 7
Connection: keep-alive

script

$ curl -D - http://localhost:8000/oops
HTTP/1.1 404 Not Found
Connection: keep-alive
Transfer-Encoding: chunked
```

## 使用你的头文件

text/plain 是站不住脚的。如果我们使用 application/json 或 text/xml 会怎么样？同时,我们的存储检索过程也可以用一点重构—一些异常的投掷&处理怎么样?来看看我们能想出什么:
```
http = require 'http'

# known mime types
[any, json, xml] = ['*/*', 'application/json', 'text/xml']

# gets a value from the db in format [value, contentType]
get = (store, key, format) ->
    value = store[key]
    throw 'Unknown key' if not value
    switch format
        when any, json then [JSON.stringify({ key: key, value: value }), json]
        when xml then ["<key>#{ key }</key>\n<value>#{ value }</value>", xml]
        else throw 'Unknown format'

store =
    foo:    'bar'
    coffee: 'script'

server = http.createServer (req, res) ->
    console.log req.method, req.url
    
    try
        key = req.url[1..]
        [value, contentType] = get store, key, req.headers.accept
        code = 200
    catch error
        contentType = 'text/plain'
        value = error
        code = 404
 
    res.writeHead code,
        'Content-Type': contentType
        'Content-Length': value.length + 1
    res.write value + '\n'
    res.end()

server.listen 8000
```

这个服务器仍然会返回一个匹配给定键的值,如果不存在则返回404。但它根据 Accept 标头将响应在 JSON 或 XML 结构中。可亲眼看一下：
```
$ curl http://localhost:8000/
Unknown key

$ curl http://localhost:8000/coffee
{"key":"coffee","value":"script"}

$ curl -H "Accept: text/xml" http://localhost:8000/coffee
<key>coffee</key>
<value>script</value>

$ curl -H "Accept: image/png" http://localhost:8000/coffee
Unknown format
```

## 你必须给回来

我们的最后一步是提供客户端存储数据的能力。我们将通过监听 POST 请求来保持 RESTiness。
```
http = require 'http'

# known mime types
[any, json, xml] = ['*/*', 'application/json', 'text/xml']

# gets a value from the db in format [value, contentType]
get = (store, key, format) ->
    value = store[key]
    throw 'Unknown key' if not value
    switch format
        when any, json then [JSON.stringify({ key: key, value: value }), json]
        when xml then ["<key>#{ key }</key>\n<value>#{ value }</value>", xml]
        else throw 'Unknown format'

# puts a value in the db
put = (store, key, value) ->
    throw 'Invalid key' if not key or key is ''
    store[key] = value

store =
    foo:    'bar'
    coffee: 'script'

# helper function that responds to the client
respond = (res, code, contentType, data) ->
    res.writeHead code,
        'Content-Type': contentType
        'Content-Length': data.length
    res.write data
    res.end()

server = http.createServer (req, res) ->
    console.log req.method, req.url
    key = req.url[1..]
    contentType = 'text/plain'
    code = 404
    
    switch req.method
        when 'GET'
            try
                [value, contentType] = get store, key, req.headers.accept
                code = 200
            catch error
                value = error
            respond res, code, contentType, value + '\n'

        when 'POST'
            value = ''
            req.on 'data', (chunk) -> value += chunk
            req.on 'end', () ->
                try
                    put store, key, value
                    value = ''
                    code = 200
                catch error
                    value = error + '\n'
                respond res, code, contentType, value

server.listen 8000
```

在一个 POST 请求中注意数据是如何接收的。通过在“数据”和“结束”请求对象的事件中附上一些处理程序，我们最终能够从客户端缓冲和保存数据。
```
$ curl -D - http://localhost:8000/cookie
HTTP/1.1 404 Not Found # ...
Unknown key

$ curl -D - -d "monster" http://localhost:8000/cookie
HTTP/1.1 200 OK # ...

$ curl -D - http://localhost:8000/cookie
HTTP/1.1 200 OK # ...
{"key":"cookie","value":"monster"}
```

## 讨论

给 http.createServer 一个函数(request，response)- >……它将返回一个服务器对象，我们可以用它来监听一个端口。给服务器与 request 和 response 的交互行为。使用 server.listen 8000 监听端口8000。

在这个问题上的API和整体信息，检查 node.js [http](http://nodejs.org/docs/latest/api/http.html) 和 [https](http://nodejs.org/docs/latest/api/https.html) 文档页面。此外，[HTTP spec](http://www.ietf.org/rfc/rfc2616.txt) 可能派上用场。

## 练习

- 在服务器和开发人员之间创建一个层，将允许开发人员做类似的事情:
```
server = layer.createServer
    'GET /': (req, res) ->
        ...
    'GET /page': (req, res) ->
        ...
    'PUT /image': (req, res) ->
        ...
```





































































































