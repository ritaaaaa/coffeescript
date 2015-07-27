# 双向客户

## 问题

你想通过网络提供持续的服务,与客户保持持续的联系。

## 方案

创建一个双向 TCP 客户机。

## In Node.js
```
net = require 'net'

domain = 'localhost'
port = 9001

ping = (socket, delay) ->
    console.log "Pinging server"
    socket.write "Ping"
    nextPing = -> ping(socket, delay)
    setTimeout nextPing, delay

connection = net.createConnection port, domain

connection.on 'connect', () ->
    console.log "Opened connection to #{domain}:#{port}"
    ping connection, 2000

connection.on 'data', (data) ->
    console.log "Received: #{data}"

connection.on 'end', (data) ->
    console.log "Connection closed"
    process.exit()
```

## 使用示例

可访问 [Bi-Directional Server](http://coffeescript-cookbook.github.io/chapters/networking/bi-directional-server)：
```
$ coffee bi-directional-client.coffee
Opened connection to localhost:9001
Pinging server
Received: You have 0 peers on this server
Pinging server
Received: You have 0 peers on this server
Pinging server
Received: You have 1 peer on this server
[...]
Connection closed
```

## 讨论

这个特殊示例发起与服务器联系并在 @connection.on 'connect'@ 处理程序中开启对话。大量的工作在一个真正的用户中，然而 @connection.on 'data'@ 处理来自服务器的输出。@ping@ 函数递归是为了说明连续与服务器通信可能被真实的用户移除。

另请参阅 [Bi-Directional Server](http://coffeescript-cookbook.github.io/chapters/networking/bi-directional-server)，[Basic Client](http://coffeescript-cookbook.github.io/chapters/networking/basic-client) 和 [Basic Server](http://coffeescript-cookbook.github.io/chapters/networking/basic-server)。

## 练习

- 为选定的目标域和基于命令行参数或配置文件的端口添加支持。






























