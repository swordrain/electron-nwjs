# 入门
* 起源于node-webkit项目
* 两者在内部架构上采用不同的方案

## NW.js
[官网](https://nwjs.io/)

安装
```
npm install -g nw
```

如果网络不佳，导致`postinstall`执行时无法正常从github上下载，可以手动下载nw在不同平台上对应的包，解压到`C:\Users\lianli\AppData\Roaming\npm\node_modules\nw\nwjs`文件夹，具体路径可以参考`C:\Users\lianli\AppData\Roaming\npm\node_modules\nw`下的`index.js`文件内容

![nw.js](https://github.com/swordrain/electron-nwjs/blob/master/image/nwjs-win.png)

## NW.js的Hello World
新建`package.json`文件
```
{
    "name": "hello-world-nwjs",
    "main": "index.html",
    "version": "1.0.0"
}
```
新建`index.html`文件

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
    <style>
        body {
            background-image: linear-gradient(45deg, #ead790 0%, #ef8c53 100%);
            text-align: center;
        }

        button {
            background: rgba(0, 0, 0, 0.4);
            box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.5);
            border-radius: 8px;
            color: white;
            padding: 1em 2em;
            border: none;
            font-weight: 100;
            font-size: 14pt;
            position: relative;
            top: 40%;
            cursor: pointer;
            outline: none;
        }

        button::hover {
            background: rgba(0, 0, 0, 0.3);
        }
    </style>
</head>

<body>
    <button onclick="sayHello()">Say Hello</button>
    <script>
        function sayHello(){
            alert('Hello World')
        }
    </script>
</body>

</html>
```

在`cmd`中进入所在路径，执行`nw`

![nwjs-helloworld](https://github.com/swordrain/electron-nwjs/blob/master/image/nwjs-helloworld.png)

`nw.js`特性

* 一套访问操作系统的API
* 使用`Node.js`
* 为不同的操作系统构建可执行文件

## Electron
它和`nw.js`的区别之一就是整合`Chromium`和`Node.js`的方式不同。在`nw.js`中，`Chromium`是直接被打补丁进去的，因此`Node.js`和`Chromium`共享了同一个`JavaScript`上下文。而`Electron`是通过`Chromium`的`Content API`以及使用了`Node.js`的`node_bindings`，它有多个独立的`JavaScript`上下文——一个是后端进程负责启动运行应用的视窗（main进程），另一个是负责具体的应用视窗（renderer进程）

另外，两者的入口文件不同，一个是html，一个是JavaScript

## Electron的Hello World
新建`package.json`
```
{
    "name": "hello-world",
    "version": "1.0.0",
    "main": "main.js"
}
```

新建`main.js`
```
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

let mainWindow = null;

app.on("window-all-closed", () => {
    if(process.platform !== 'darwin') app.quit();
});

app.on('ready', () => {
    mainWindow = new BrowserWindow();
    mainWindow.loadURL(`file://${__dirname}/index.html`);
    mainWindow.on("closed", ()=> {
        mainWindow = null;
    });
});
```

新建`index.html`
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Hello World</title>
    <style>
        body {
            background-image: linear-gradient(45deg, #ead790 0%, #ef8c53 100%);
            text-align: center;
        }

        button {
            background: rgba(0, 0, 0, 0.4);
            box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.5);
            border-radius: 8px;
            color: white;
            padding: 1em 2em;
            border: none;
            font-weight: 100;
            font-size: 14pt;
            position: relative;
            top: 40%;
            cursor: pointer;
            outline: none;
        }

        button::hover {
            background: rgba(0, 0, 0, 0.3);
        }
    </style>
</head>

<body>
    <button onclick="sayHello()">Say Hello</button>
    <script>
        function sayHello(){
            alert('Hello World')
        }
    </script>
</body>

</html>
```

进入目录后执行
```
electron .
```

!(electron-helloworld)[https://github.com/swordrain/electron-nwjs/blob/master/image/electron-helloworld.png]

`electron`执行后默认带菜单

`electron`特性
* 支持多视窗，每个视窗都有独立的上下文
* 通过shell和screen API整合了桌面操作系统的特性
* 支持获取计算机电源状态
* 支持阻止操作系统进入省电模式
* 支持创建托盘应用
* 支持创建菜单和菜单项
* 支持为应用增加全局键盘快捷键
* 支持通过应用更新来自动更新应用代码
* 支持汇报程序崩溃
* 支持自定义Dock菜单项
* 支持操作系统通知
* 支持为应用创建启动安装器

## 一些使用electron和nwjs的产品
* Slack
* Light Table
* Game Dev Tycoon
* Gitter
* Macaw
* Hyper

# 首款桌面应用搭建基础架构
