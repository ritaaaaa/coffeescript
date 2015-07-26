# 基本的服务器 

## 问题

你想在网络上提供一个服务器。

## 方案

创建一个基本的 TCP 服务器。

## In Node.js
```
net = require 'net'

domain = 'localhost'
port = 9001

server = net.createServer (socket) ->
    console.log "Received connection from #{socket.remoteAddress}"
    socket.write "Hello, World!\n"
    socket.end()

console.log "Listening to #{domain}:#{port}"
server.listen port, domain
```

## 使用示例

可访问 [Basic Client](http://coffeescript-cookbook.github.io/chapters/networking/basic-client):
```
$ coffee basic-server.coffee
Listening to localhost:9001
Received connection from 127.0.0.1
Received connection from 127.0.0.1
[...]
```

## 讨论

函数将为每个客户端新连接的新插口传递给 @net.createServer@。基本的服务器与访客只进行简单地交互，但是复杂的服务器会将插口连上一个专用的处理程序,然后返回等待下一个用户的任务。

另请参阅 [Basic Client](http://coffeescript-cookbook.github.io/chapters/networking/basic-client)，[Bi-Directional Server](http://coffeescript-cookbook.github.io/chapters/networking/bi-directional-client) 和 [Bi-Directional Client](http://coffeescript-cookbook.github.io/chapters/networking/bi-directional-client)。

## 练习

- 为选定的目标域和基于命令行参数或配置文件的端口添加支持。







