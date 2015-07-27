# 双向服务器

## 问题

你想通过网络提供持续的服务，与客户保持持续的联系。

## 方案

创建一个双向 TCP 服务器。

## In Node.js
```
net = require 'net'

domain = 'localhost'
port = 9001

server = net.createServer (socket) ->
    console.log "New connection from #{socket.remoteAddress}"

    socket.on 'data', (data) ->
        console.log "#{socket.remoteAddress} sent: #{data}"
        others = server.connections - 1
        socket.write "You have #{others} #{others == 1 and "peer" or "peers"} on this server"

console.log "Listening to #{domain}:#{port}"
server.listen port, domain
```

## 使用示例

可访问 [Bi-Directional Client](http://coffeescript-cookbook.github.io/chapters/networking/bi-directional-client)：
```
$ coffee bi-directional-server.coffee
Listening to localhost:9001
New connection from 127.0.0.1
127.0.0.1 sent: Ping
127.0.0.1 sent: Ping
127.0.0.1 sent: Ping
[...]
```

## 讨论

大部分工作在 @socket.on 'data'@ ，处理所有的输入端。真正的服务器可能会将数据传给另一个函数处理并生成原处理程序的任何响应。

## 练习

- 为选定的目标域和基于命令行参数或配置文件的端口添加支持。























