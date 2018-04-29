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
```json
{
    "name": "hello-world-nwjs",
    "main": "index.html",
    "version": "1.0.0"
}
```
新建`index.html`文件

```html
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
```json
{
    "name": "hello-world",
    "version": "1.0.0",
    "main": "main.js"
}
```

新建`main.js`
```js
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
```html
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

![electron-helloworld](https://github.com/swordrain/electron-nwjs/blob/master/image/electron-helloworld.png)

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
```json
{
    "name": "lorikeet",
    "version": "1.0.0",
    "main": "index.html"
}
```

`index.html`的内容
```html
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
```json
{
    "name": "lorikeet",
    "version": "1.0.0",
    "main": "main.js"
}
```

`main.js`内容
```js
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
```html
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
```css
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
```html
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
```html
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
```js
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
```html
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
```js
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
```html
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
```css
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
```js
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

#构建首款桌面应用
## 重构
将`app.js`拆分成三份
```js
//fileSystem.js
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

module.exports = {
    getUsersHomeFolder,
    getFilesInFolder,
    inspectAndDescribeFiles
}


//userInterface.js
let document;

function displayFile(file) {
    const mainArea = document.getElementById('main-area');
    const template = document.querySelector('#item-template');
    let clone = document.importNode(template.content, true);
    clone.querySelector('img').src = `images/${file.type}.svg`;
    clone.querySelector('.filename').innerText = file.file;
    mainArea.appendChild(clone);
}

function displayFiles(err, files) {
    if (err) {
        return alert('Sorry, we could not display your files');
    }
    files.forEach(displayFile);
}

function bindDocument(window) {
    if (!document) {
        document = window.document;
    }
}

module.exports = {
    bindDocument,
    displayFiles
}

//app.js
const fileSystem = require('./fileSystem');
const userInterface = require('./userInterface');

function main() {
    userInterface.bindDocument(window);
    const folderPath = fileSystem.getUsersHomeFolder();
    fileSystem.getFilesInFolder(folderPath, (err, files) => {
        if (err) {
            return alert('Sorry, we could not load your home folder');
        }
        fileSystem.inspectAndDescribeFiles(folderPath, files, userInterface.displayFiles);
    })
}

main();
```
`index.html`的修改
```html
<div id="toolbar">
    <div id="current-folder">
        <script>
            document.write(fileSystem.getUsersHomeFolder());
        </script>
    </div>
</div>
```

## 处理文件夹双击操作
修改`userInterface.js`
```Javascript
const fileSystem = require('./fileSystem'); //增加该模块
let document;


function displayFolderPath(folderPath) { //显示当前文件夹路径
    document.getElementById('current-folder').innerText = folderPath;
}

function clearView() { //移除main-area div元素中的内容
    const mainArea = document.getElementById('main-area');
    let firstChild = mainArea.firstChild;
    while (firstChild) {
        mainArea.removeChild(firstChild);
        firstChild = mainArea.firstChild;
    }
}

function loadDirectory(folderPath) { //loadDirectory修改当前文件夹路径并更新主区域中的内容
    return function (window) {
        if (!document) {
            document = window.document;
        }
        
        displayFolderPath(folderPath);
        fileSystem.getFilesInFolder(folderPath, (err, files) => {
            clearView();
            if (err) {
                return alert('Sorry, you could not load your folder');
            }
            fileSystem.inspectAndDescribeFiles(folderPath, files, displayFiles);
        });
    }
}

function displayFile(file) {
    const mainArea = document.getElementById('main-area');
    const template = document.querySelector('#item-template');
    let clone = document.importNode(template.content, true);
    clone.querySelector('img').src = `images/${file.type}.svg`;

    if (file.type === 'directory') { //文件夹增加双击事件
        clone.querySelector('img').addEventListener('dblclick', () => {
            loadDirectory(file.path)();
        }, false);
    }

    clone.querySelector('.filename').innerText = file.file;
    mainArea.appendChild(clone);
}

function displayFiles(err, files) {
    if (err) {
        return alert('Sorry, we could not display your files');
    }
    files.forEach(displayFile);
}

function bindDocument(window) {
    if (!document) {
        document = window.document;
    }
}

module.exports = {
    bindDocument,
    displayFiles,
    loadDirectory //暴露出去
}
```

修改`app.js`
```
const fileSystem = require('./fileSystem');
const userInterface = require('./userInterface');

function main() {
    userInterface.bindDocument(window);
    const folderPath = fileSystem.getUsersHomeFolder();
    userInterface.loadDirectory(folderPath)(window);
}

window.onload = main; //要等到加载完成后才执行，否则会找不到dom元素
```

修改`index.html`，去除`<div id="current-folder"></div>`之间的`<script>`内容

![electron-folder-drilldown](https://github.com/swordrain/electron-nwjs/blob/master/image/electron-folder-drilldown.png)

## 实现快速搜索
在`<div id="current-folder">`后添加`<input type="search" id="search" results="5" placeholder="Search" />`

修改`app.css`
```css
#search {
    float: right;
    padding: .5em;
    min-width: 10em;
    border-radius: 3em;
    margin: 2em 1em;
    border: none;
    outline: none;
}
```

![search](https://github.com/swordrain/electron-nwjs/blob/master/image/search.png)

安装
```
npm install --save lunr@0.7.2
#安装最新版本会导致代码不匹配
```

新建`search.js`
```js
const lunr = require('lunr');
let index;

function resetIndex() {
    index = lunr(function () {
        this.field('file');
        this.field('type');
        this.ref('path');
    });
}

function addToIndex(file) {
    index.add(file);
}

function find(query, cb) {
    if (!index) {
        resetIndex();
    }
    const results = index.search(query);
    cb(results);
}

module.exports = {
    addToIndex,
    find,
    resetIndex
}
```

修改`userInterface.js`
```js
const search = require('./search');
...

function loadDirectory(folderPath) {
    return function (window) {
        if (!document) {
            document = window.document;
        }
        search.resetIndex(); //重置
        displayFolderPath(folderPath);
        fileSystem.getFilesInFolder(folderPath, (err, files) => {
            clearView();
            if (err) {
                return alert('Sorry, you could not load your folder');
            }
            fileSystem.inspectAndDescribeFiles(folderPath, files, displayFiles);
        });
    }
}

function displayFile(file) {
    const mainArea = document.getElementById('main-area');
    const template = document.querySelector('#item-template');
    let clone = document.importNode(template.content, true);
    search.addToIndex(file); //将文件添加到搜索索引中
    clone.querySelector('img').src = `images/${file.type}.svg`;
    clone.querySelector('img').setAttribute('data-filePath', file.path);

    if (file.type === 'directory') {
        clone.querySelector('img').addEventListener('dblclick', () => {
            loadDirectory(file.path)();
        }, false);
    }

    clone.querySelector('.filename').innerText = file.file;
    mainArea.appendChild(clone);
}
...

function bindSearchField(cb) {
    document.getElementById('search').addEventListener('keyup', cb, false);
}

function filterResults(results) { //获取搜索结果中的文件路径用于比对
    const validFilePaths = results.map(result => result.ref);
    const items = document.getElementsByClassName('item');
    for (let item of items) {
        let filePath = item.getElementsByTagName('img')[0].getAttribute('data-filepath');
        if (validFilePaths.indexOf(filePath) !== -1) {
            item.style = null;
        } else {
            item.style = 'display:none';
        }
    }
}

function resetFilter() {
    const items = document.getElementsByClassName('item');
    for (let item of items) {
        item.style = null;
    }
}

module.exports = {
    bindDocument,
    displayFiles,
    loadDirectory,
    bindSearchField,
    filterResults,
    resetFilter
}
```

修改`app.js`
```js
function main() {
    userInterface.bindDocument(window);
    const folderPath = fileSystem.getUsersHomeFolder();
    userInterface.loadDirectory(folderPath)(window);

    userInterface.bindSearchField( event => { //监听搜索框的变化
        const query = event.target.value;
        if (query === '') {
            userInterface.resetFilter();
        } else {
            search.find(query, userInterface.filterResults);
        }
    });
}
```

![filter](https://github.com/swordrain/electron-nwjs/blob/master/image/filter.png)

## 改进导航
修改`userInterface.js`
```js
const path = require('path');
...

function displayFolderPath(folderPath) {
    document.getElementById('current-folder').innerHTML = convertFolderPathIntoLinks(folderPath);
    bindCurrentFolderPath();
}
...

function convertFolderPathIntoLinks(folderPath) {
    const folders = folderPath.split(path.sep);
    const contents = [];
    let pathAtFolder = '';
    folders.forEach( folder => {
        pathAtFolder += folder + path.sep;
        contents.push(`<span class="path" data-path="${pathAtFolder.slice(0, -1)}">${folder}</span>`);
    });
    return contents.join(path.sep).toString();
}

function bindCurrentFolderPath() {
    const load = event => {
        const folderPath = event.target.getAttribute('data-path');
        loadDirectory(folderPath)();
    };
    const paths = document.getElementsByClassName('path');
    for(let path of paths) {
        path.addEventListener('click', load, false);
    }
}
```

修改`app.css`
```css
span.path:hover {
    opacity: .7;
    cursor: pointer;
}
```

![nav-link](https://github.com/swordrain/electron-nwjs/blob/master/image/nav-link.png)

## 使用默认应用打开文件
修改`userInterface.js`
```js
function displayFile(file) {
    const mainArea = document.getElementById('main-area');
    const template = document.querySelector('#item-template');
    let clone = document.importNode(template.content, true);
    search.addToIndex(file);
    clone.querySelector('img').src = `images/${file.type}.sve`;
    clone.querySelector('img').setAttribute('data-filePath', file.path);
    if (file.type === 'directory') {
        clone.querySelector('img').addEventListener('dblclick', () => {
            loadDirectory(file.path)();
        }, false);
    } else {
        clone.querySelector('img').addEventListener('dblclick', () => {
            fileSystem.openFile(file.path);
        }, false);
    }
    clone.querySelector('.filename').innerText = file.file;
    mainArea.appendChild(clone);
}
```

修改`fileSystem.js`
```js
let shell;
if (process.versions.electron) {
    shell = require('electron').shell;
} else {
    shell = window.require('nw.gui').Shell
}

function openFile(filePath) {
    shell.openItem(filePath);
}
```

修改`app.css`
```css
span.path:hover, img:hover {
    opacity: .7;
    cursor: pointer;
}
```

# 分发应用
## 创建应用图标
### Mac OS
Mac OS的图标格式为ICNS，包含以下分辨率
* 16px
* 32px
* 128px
* 256px
* 512px

可以使用`iConvert Icons`来帮助转换

### Windows
Windows使用ICO格式，可以使用`iConvert Icons`也可以使用`icoconverter.com`在线生成

### Linux
访问`http://standards.freedesktop.org/desktop-entry-spec/latest`来获取标准

`xxx.desktop`是一个配置文件，包含了应用名、运行路径、图标位置等配置信息

## 打包
### NW.js
```
npm install nw-builder -g
nwbuild . -o ./build -p win64,osx64,linux64
```

### Electron
```
npm install electron-builder electron --save-dev
```

检查`package.json`包含名称、描述、版本号、作者、构建配置、打包和分发脚本，如
```json
{
    "name": "lorikeet",
    "version": "1.0.0",
    "main": "main.js",
    "author": "lianli",
    "description": "A file explorer",
    "dependencies": {
        "async": "^2.6.0",
        "lunr": "^0.7.2",
        "osenv": "^0.1.5"
    },
    "scripts": {
        "pack": "build",
        "dist": "build"
    },
    "devDependencies": {
        "electron": "^1.4.14",
        "electron-builder": "^11.4.4"
    },
    "build" : {}
}
```

使用命令
```
npm run pack
```

## 设置应用图标
### Mac OS
在应用上右键选择`获取信息Get Info`，将图标文件拖拽到左上角应用图标的位置

### Windows
使用`Resource Hacker`(http://angusj.com/resourcehacker)

在`nw-builder`中，设置`package.json`并重新构建应用
```json
    "icon": "icon.png"
```

### Linux
根据不同的桌面环境，如果是`Gnome`，右击图标，选择属性命令，单击左上角图标，选择图片然后确认

# 内部机制

