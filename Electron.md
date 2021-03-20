# Electron 初体验

#### 安装
1.按照官方教程，我们需要做一个前置工作, 创建项目目录，并初始化`package.json`
```
mkdir electron
cd electron

npm init -y
```

2.安装`electron`
```
npm i electron -D
```
3.在安装过程中，由于国内的网络环境，会安装不了，后面会单独说

#### 初始化项目
需要创建几个文件目录结构如下
```
electron-demo/
├── package.json
├── main.js
└── index.html
```
1.修改`package.json`
```
{
  "name": "electron-demo",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "electron ."
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "electron": "^9.1.2"
  }
}
```
2.新建`index.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello Electron!</h1>
</body>
</html>
```
3.新建`main.js`
```
const { app, BrowserWindow } = require('electron')

function createWindow () {   
  // 创建浏览器窗口
  let win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true
    }
  })

  // 加载index.html文件
  win.loadFile('index.html')
}

app.whenReady().then(createWindow)
```
#### 启动应用
```
npm start
```
#### 打包
1.安装打包工具`electron-builder`,可能会有网络问题，后面会说怎么解决
```
npm i electron-builder -D
```
2.修改`package.json`,添加下面的内容, appID可以随便写，category则是所属类别，当然也可以随便写（就目前初体验来说）
```
"build": {
  "appId": "your.id",
  "mac": {
    "category": "your.app.category.type"
  }
}
```
`scripts`里加下面两行
```
"scripts": {
  "pack": "electron-builder --dir",
  "dist": "electron-builder"
}
```