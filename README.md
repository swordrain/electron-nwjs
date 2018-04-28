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
安装
```
npm install -g nw
```

当网络问题无法正常下载的时候可以参考和`nwjs`一样的方式，下载压缩包，找到`C:\Users\lianli\AppData\Roaming\npm\node_modules\electron\index.js`文件中指向的路径，将压缩包解压到该路径下

![electron-install](https://github.com/swordrain/electron-nwjs/blob/master/image/electron-install.png)

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
## 创建nw.js应用
```
mkdir lorikeet-nwjs
cd lorikeet-nwjs
type nul>package.json
type nul>index.html
```

`package.json`的内容
```
{
    "name": "lorikeet",
    "version": "1.0.0",
    "main": "index.html"
}
```

`index.html`的内容
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lorikeet</title>
</head>
<body>
    <h1>Welcome to Lorikeet</h1>
</body>
</html>
```

窗口的标题是和`<title>`里的内容一致

![lorikeet-nwjs-1](https://github.com/swordrain/electron-nwjs/blob/master/image/lorikeet-nwjs-1.png)

## 创建electron应用
```
mkdir lorikeet-electron
cd lorikeet-electron
type nul>package.json
type nul>main.js
type nul>index.html
```

`package.json`内容
```
{
    "name": "lorikeet",
    "version": "1.0.0",
    "main": "main.js"
}
```

`main.js`内容
```
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;

let mainWindow = null;

app.on('window-all-closed', () => {
    if(process.platform !== 'darwin') {
        app.quit();
    }
});

app.on('ready', () => {
    mainWindow = new BrowserWindow();
    mainWindow.loadURL(`file://${app.getAppPath()}/index.html`);
    mainWindow.on('closed', () => { mainWindow = null });
});
```

`index.html`内容
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lorikeet</title>
</head>
<body>
    <h1>Welcome to Lorikeet</h1>
</body>
</html>
```

![lorikeet-electron-1](https://github.com/swordrain/electron-nwjs/blob/master/image/lorikeet-electron-1.png)

## 启动界面
新建`app.css`
```
body {
    padding: 0;
    margin: 0;
}

#toolbar {
    position: absolute;
    background: red;
    width: 100%;
    padding: 1em;
}

#current-folder {
    float: left;
    color: white;
    background: rgba(0, 0, 0, 0.2);
    padding: .5em 1em;
    min-width: 10em;
    border-radius: .2em;
}
```

`index.html`中添加
```
<body>
    <div id="toolbar">
        <div id="current-folder"></div>
    </div>
</body>
```

![toolbar](https://github.com/swordrain/electron-nwjs/blob/master/image/toolbar.png)

安装`osenv`
```
npm install osenv --save
```

修改`index.html`
```
<div id="toolbar">
    <div id="current-folder">
        <script>
            document.write(require('osenv').home());
        </script>
    </div>
</div>
```

![home-folder](https://github.com/swordrain/electron-nwjs/blob/master/image/home-folder.png)

新建`app.js`
```
const osenv = require('osenv');
const fs = require('fs');

function getUsersHomeFolder() {
    return osenv.home();
}

function getFilesInFolder(folderPath, cb) {
    fs.readdir(folderPath, cb);
}

function main() {
    const folderPath = getUsersHomeFolder();
    getFilesInFolder(folderPath, (err, files) => {
        if (err) {
            return alert('Sorry, we could not load your home folder');
        }
        files.forEach(file => {
            console.log(`${folderPath}/${file}`);
        })
    })
}

main();
```

修改`index.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lorikeet</title>
    <link rel="stylesheet" href="app.css" />
    <script src="app.js"></script>
</head>
<body>
    <div id="toolbar">
        <div id="current-folder">
            <script>
                document.write(getUsersHomeFolder());
            </script>
        </div>
    </div>
</body>
</html>
```

`electron`的菜单项里有`View - Toggle Developer Tools`，可以显示`debug`信息

![electron-console](https://github.com/swordrain/electron-nwjs/blob/master/image/electron-console.png)

安装`async`模块
```
npm install --save async
```

修改`app.js`
```
const osenv = require('osenv');
const fs = require('fs');
const async = require('async');
const path = require('path');

function getUsersHomeFolder() {
    return osenv.home();
}

function getFilesInFolder(folderPath, cb) {
    fs.readdir(folderPath, cb);
}

function inspectAndDescribeFile(filePath, cb) {
    let result = {
        file: path.basename(filePath),
        path: filePath,
        type: ''
    };
    fs.stat(filePath, (err, stat) => {
        if (err) {
            cb(err);
        } else {
            if (stat.isFile()) {
                result.type = 'file';
            }
            if (stat.isDirectory()) {
                result.type = 'directory';
            }
            cb(err, result);
        }
    });
}

function inspectAndDescribeFiles(folderPath, files, cb) {
    async.map(files, (file, asyncCb) => {
        let resolvedFilePath = path.resolve(folderPath, file);
        inspectAndDescribeFile(resolvedFilePath, asyncCb);        
    }, cb);
}

function displayFiles(err, files){
    if(err) {
        return alert('Sorry, we could not display your files');
    }
    files.forEach(file => { console.log(file); });
}


function main() {
    const folderPath = getUsersHomeFolder();
    getFilesInFolder(folderPath, (err, files) => {
        if (err) {
            return alert('Sorry, we could not load your home folder');
        }
        inspectAndDescribeFiles(folderPath, files, displayFiles);
    })
}

main();
```

![electron-console-content](https://github.com/swordrain/electron-nwjs/blob/master/image/electron-console-content.png)

修改`index.html`
```
<body>
    <template id="item-template">
        <div class="item">
            <img class="icon" />
            <div class="filename"></div>
        </div>
    </template>
    <div id="toolbar">
        <div id="current-folder">
            <script>
                document.write(getUsersHomeFolder());
            </script>
        </div>
    </div>
    <div id="main-area"></div>
</body>
```

修改`app.css`
```
body {
    padding: 0;
    margin: 0;
}

#toolbar {
    top: 0;
    position: fixed;
    background: red;
    width: 100%;
    z-index: 2;
}

#current-folder {
    float: left;
    color: white;
    background: rgba(0, 0, 0, 0.2);
    padding: .5em 1em;
    min-width: 10em;
    border-radius: .2em;
    margin: 1em;
}

#main-area {
    clear: both;
    margin: 2em;
    margin-top: 3em;
    z-index: 1;
}

.item {
    position: relative;
    float: left;
    padding: 1em;
    margin: 1em;
    width: 5em;
    height: 6em;
    text-align: center;
}

.item .filename {
    padding-top: 1em;
    font-size: 10pt;
}
```

修改`app.js`
```
function displayFile(file) {
    const mainArea = document.getElementById('main-area');
    const template = document.querySelector('#item-template');
    let clone = document.importNode(template.content, true);
    clone.querySelector('img').src = `images/${file.type}.svg`;
    clone.querySelector('.filename').innerText = file.file;
    mainArea.appendChild(clone);
}

function displayFiles(err, files){
    if(err) {
        return alert('Sorry, we could not display your files');
    }
    files.forEach(displayFile);
}
```

![lorikeet-effect-1](https://github.com/swordrain/electron-nwjs/blob/master/image/lorikeet-effect-1.png)
