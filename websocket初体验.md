# WebSocket初体验

#### 纸上得来终觉浅

#### 实践

以node搭建服务端，安装`express`、`ws`

```
npm i express ws
```

创建`server.js`文件

```javascript
const express = require('express')
const SocketServer = require('ws').Server


const PORT = 3000
const server = express().listen(PORT, () => console.log(`Listen on ${PORT}`))

const wss = new SocketServer({server})

wss.on('connection', (ws) => {
    console.log('Client connected')

    // 定时向客户端发送消息
    const sendHelloAndNowTime = setInterval(() => {
        ws.send(`hello 不动心 ${String(new Date())}`)
    }, 1000)

    ws.on('message', (data) => {
        // 向所有客户端发送消息
        wss.clients.forEach(client => {
            client.send(data)
        })
        // ws.send(data)
    })

    ws.on('close', () => {
        clearInterval(sendHelloAndNowTime)
        console.log('Close connected')
    })
})
```

创建`index.html`和`index.js`

```
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="root"></div>
    <script src="index.js"></script>
</body>
</html>
```

```
// index.js
// 改变页面上的元素
const root = document.querySelector('#root')

let ws = new WebSocket('ws://127.0.0.1:3000')

ws.onopen = () => {
    console.log('open connection')
}

ws.onmessage = ({data}) => {
    // console.log(data)
    root.innerHTML += `${data} </br>`
}

ws.onclose = () => {
    console.log('close connection')
}
```



参见这篇文章 https://juejin.im/post/6876301731966713869

#### socket.io Get Start

创建`app.js`

```
const { isObject } = require('util')

const app = require('express')()
const server = require('http').createServer(app)
const io = require('socket.io')(server)


app.get('/', (req, res) => {
    res.sendFile(__dirname + '/index.html')
})

io.on('connection', (socket) => {
    console.log('用户已连接')

    socket.on('chat message', (msg) => {
        console.log(msg)

        // 向所有用户广播
        io.emit('chat message', msg)
    })

    // 向某个socket之外的其他socket发送
    // socket.broadcast.emit('hi')

    socket.on('disconnect', () => {
        console.log('用户已断开连接')
    })
})

server.listen(3000, () => console.log('Listening on *: 3000'))
```

创建`index.html`

```
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      body { font: 13px Helvetica, Arial; }
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
      form input { border: 0; padding: 10px; width: 90%; margin-right: 0.5%; }
      form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages li { padding: 5px 10px; }
      #messages li:nth-child(odd) { background: #eee; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>
  </body>
</html>
```

安装`socket.io`

```
npm i socket.io
```

