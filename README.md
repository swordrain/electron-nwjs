![cover](https://img11.360buyimg.com/n1/jfs/t17152/89/1135528726/344771/5944fb5a/5abca7aeN33b58fbc.jpg)

[github](https://github.com/paulbjensen/cross-platform-desktop-applications/)

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
## NW.js
`NW.js`将`Node.js`和`Chromium`(使用`Blink`作为渲染引擎)通过`V8`引擎整合起来

* 它们使用同一份V8实例
    1. 加载`Node.js`和`Chromium`
    2. `Node.js`的js上下文复制到`Chromium`的js上下文中
* 将主要的事件循环集成
    1. `Chromium`的事件循环被调整为使用一个自定义版本的`MessagePump`，它是构建在`libuv`（`Node.js`所用）之上的
* 它们之间桥接`JavaScript`上下文
    1. `Node.js`的`start`函数和`Chromium`的渲染进程进行集成

## Electron
* 使用`libchromiumcontent`代码库加载`Chromium`的`content`模块
* Electron的组件
    * App - 处理启动时的加载工作
    * Browser - 处理前端部分的交互
    * Renderer - 运行在`renderer`进程中
    * Common - 工具类代码

小结
* 在`NW.js`中，`Node.js`和`Blink`共享`JavaScript`上下文，可以在多视窗间共享数据
* 共享`JavaScript`状态意味着在同一个`NW.js`的多视窗应用中，可以共享同一个状态
* `NW.js`使用了编译后的`Chromium`版本以及自定义绑定，而`Electron`使用了`Chromium`中的API将`Chromium`和`Node.js`整合在一起
* `Electron`将前后端的`JavaScript`上下文进行了隔离
* 当要在`Electron`应用的前后端进行数据共享时，需要通过`ipcMain`和`ipcRenderer`两个API进行消息传递来实现

# 自定义桌面应用的外观
## 视窗的尺寸和模式
### NW.js
通过`package.json`声明
```json
{
    "name": "hello-world-nwjs",
    "main": "index.html",
    "version": "1.0.0",
    "window": {
        "width": 300,
        "height": 200,
        "max_width": 1024,
        "max_height": 800,
        "min_width": 200,
        "min_height": 100
    }
}
```

通过`JavaScript`代码修改
```js
const gui = require('nw.gui');
const win = gui.Window.get();
win.width = 1024;
win.height = 768;
win.x = 400;
win.y = 500;
```

### Electron
在入口`JavaScript`文件里指定
```js
app.on('ready', () => {
    mainWindow = new BrowserWindow({
        useContentSize: true, //视窗宽高根据内容来设定
        resizable: true,
        width: 400, 
        height: 200,
        minWidth: 300,
        minHeight: 150,
        maxWidth: 600,
        maxHeight: 450,
        x: 10,
        y: 10
    });
    mainWindow.loadURL(`file://${__dirname}/index.html`);
    mainWindow.on("closed", ()=> {
        mainWindow = null;
    });
});
```

## 全屏
### NW.js
通过`package.json`指定
```json
{
    "window": {
        "fullscreen": true
    }
}
```

通过`JavaScript`代码
```js
const gui = require('nw.gui');
const win = gui.Window.get();
win.enterFullscreen();
win.leaveFullscreen();
```

### Electron
传递参数
```js
mainWindow = new BrowserWindow({
    fullscreen: true
});
```

通过`JavaScript`代码
```js
const remote = require('electron').remote;
const win = remote.getCurrentWindow();
if (win.isFullScreen()) {
    win.setFullScreen(false);
} else {
    win.setFullScreen(true);
}
```

## 无边框应用
### NW.js
在`package.json`中设置
```json
{
    "window": {
        "frame": false,
        "transparent": true 
    }
}
```

![noframe](https://github.com/swordrain/electron-nwjs/blob/master/image/noframe.png)

默认情况下，无边框应用不能再被拖动，需要给`HTML`元素（比如body）设置
```css
body {
    -webkit-app-region: drag;
}
/* 交互元素不设置 */
button, select {
    -webkit-app-region: no-drag;
}
/* 内容元素的设置 */
p, img {
    -webkit-app-region: no-drag;
    -webkit-user-select: all;
}
```

### Electron
```js
mainWindow = new BrowserWindow({
    frame: false,
    transparent: true
});
```

## kiosk模式
该模式限定了用户的操作，想象一下自助终端机，应用直接全屏，不能被关闭，也不能做别的事情

### NW.js
```json
{
    "window": {
        "kiosk": true
    }
}
```

通过`JavaScript`代码退出`kiosk`模式
```js
const gui = require('nw.gui');
const win = gui.Window.get();
win.leaveKioskMode();
```

### Electron
```js
mainWindow = new BrowserWindow({
    kiosk: true
});
```

通过代码修改`kiosk`状态
```js
const remote = require('electron').remote;
const win = remote.getCurrentWindow();
win.setKiosk(false);
win.setKiosk(true);
```

# 创建托盘应用
在windows和linux中，托盘应用只能使用图标，在Mac OS中，可以使用图标或文本

## NW.js
```js
const gui = require('nw.gui');
const tray = new gui.Tray({
    //title: 'My tray app'
    icon: "icon@2x.png"
});
const notes = [
    {
        title: 'todo list',
        contents: 'grocery shopping\npick up kids\nsend birthday party invites'
    }, {
        title: 'grocery list',
        contents: 'Milk\nEggs\nButter'
    }
];
const menu = new gui.Menu();
notes.forEach(note => {
    menu.append(new gui.MenuItem({
        label: note.title,
        click: () => { alert(note.contents) }
    }));
});
tray.menu = menu;
```

![tray](https://github.com/swordrain/electron-nwjs/blob/master/image/tray.png)

## Electron
在`main.js`中修改
```js
const electron = require("electron");
const app = electron.app;
const BrowserWindow = electron.BrowserWindow;
const Tray = electron.Tray;
const Menu = electron.Menu;

let appIcon = null;
let mainWindow = null;

const notes = [
    {
        title: 'todo list',
        contents: 'grocery shopping\npick up kids\nsend birthday party invites'
    }, {
        title: 'grocery list',
        contents: 'Milk\nEggs\nButter'
    }
];

function addNoteToMenu(note) {
    return {
        label: note.title,
        type: 'normal',
        click: () => { mainWindow.webContents.send('displayNotes', note); } //发送数据
    }
}

app.on("window-all-closed", () => {
    if (process.platform !== 'darwin') app.quit();
});

app.on('ready', () => {
    appIcon = new Tray('icon@2x.png');
    let contextMenu = Menu.buildFromTemplate(notes.map(addNoteToMenu));
    appIcon.setToolTip('Notes app');
    appIcon.setContextMenu(contextMenu);

    mainWindow = new BrowserWindow({});
    mainWindow.loadURL(`file://${__dirname}/index.html`);
    mainWindow.on("closed", () => {
        mainWindow = null;
    });
});
```

在`app.js`中响应
```js
function displayNote(event, note) {
    console.log(note)
}
const ipc = require('electron').ipcRenderer;
ipc.on('displayNote', displayNote);
```

# 应用菜单和上下文菜单
## 应用菜单
### NW.js为Mac OS创建菜单
```js
const gui = require('nw.gui');
const mb = new gui.Menu({type: 'menubar'});
mb.createMacBuiltin('Mac app menu example');
gui.Window().get().menu = mb;
```

### Electron为Mac OS创建菜单
```js
const electron = require('electron');
const Menu = electron.remote.Menu;
const name = electron.remote.app.getName();

const template = [{
    label: '',
    submenu: [{
        label: 'About ' + name,
        role: 'about'
    }, {
        type: 'sepatator'
    }, {
        label: 'Quit',
        accelerator: 'Command+Q',
        click: electron.remote.app.quit
    }]
}];

const menu = Menu.buildFromTemplate(template);
Menu.setAppMenu(menu);
```

可以使用第三方库
```
npm install electron-default-menu --save
```

```js
const electron = require('electron');
const Menu = electron.remote.Menu;
const defaultMenu = require('electron-default-menu');

const menu = Menu.buildFromTemplate(defaultMenu());
Menu.setAppMenu(menu);
```

首个菜单项的名字无论设置，都是应用的名字。如果想修改，参考`https://electronjs.org/docs/api/menu#main-menus-name`

### 为Windows和Linux创建菜单
使用`NW.js`
```js
const gui = require('nw.gui');
const menuBar = new gui.Menu({ type: 'menubar' });
const fileMenu = new gui.MenuItem({ label: 'File' });

const sayHelloMenuItem = new gui.MenuItem({
    label: 'Say hello',
    click: () => { alert('Hello') }
});
const quitAppMenuItem = new gui.MenuItem({
    label: 'Quit the app',
    click: () => { process.exit(0); }
});

const fileMenuSubMenu = new gui.Menu();
fileMenuSubMenu.append(sayHelloMenuItem);
fileMenuSubMenu.append(quitAppMenuItem);
fileMenu.submenu = fileMenuSubMenu;

menuBar.append(fileMenu);
gui.Window.get().menu = menuBar;
```

![windows-menu](https://github.com/swordrain/electron-nwjs/blob/master/image/windows-menu.png)

使用`Electron`
在`index.html`引用的`app.js`里
```js
const electron = require('electron');
const Menu = electron.remote.Menu;

const sayHello = () => { alert('Hello'); };

const quitTheApp = () => { electron.remote.app.quit(); };

const template = [
    {
        label: 'File',
        submenu: [
            {
                label: 'Say Hello',
                click: sayHello
            },
            {
                label: 'Quit the app',
                click: quitTheApp
            }
        ]
    }
];

const menu = Menu.buildFromTemplate(template);
Menu.setApplicationMenu(menu);    
```

## 判断操作系统载入菜单
```js
const os = require('os');
if (os.platform() === 'darwin') {
    loadMenuForMacOS();
} else {
    loadMenuForWindowsAndLinux();
}
```

## 上下文菜单
### NW.js
```js
 const gui = require('nw.gui');
function initialize(window, gui) {
    if (!document) document = window.document;
    const menu = new gui.Menu();
    
    menu.append(new gui.MenuItem({
        icon: 'image.png',
        label: 'Insert image',
        click: () => {}
    }));

    menu.append(new gui.MenuItem({
        icon: 'youtube.png',
        label: 'Insert video',
        click: () => {}
    }));
    document.querySelector('#app').addEventListener('contextmenu', event => {
        event.preventDefault();
        menu.popup(event.x, event.y);
        return false;
    });
}

window.onload = function() {
    initialize(window, gui);
}
```

![contextmenu](https://github.com/swordrain/electron-nwjs/blob/master/image/contextmenu.png)

### Electron
读一下[源码](https://github.com/paulbjensen/cross-platform-desktop-applications/tree/master/chapter-09/cirrus-electron)，了解`electron`是如何在`main`进程和`renderer`进程之间通过`ipcMain`和`ipcRenderer`进行消息传递

```js
const electron = require('electron');
const Menu = electron.remote.Menu;
const MenuItem = electron.remote.MenuItem;
const menu = new Menu();
menu.append(new MenuItem({label: 'Insert image', click: () => {}}));
menu.append(new MenuItem({label: 'Insert video', click: () => {}}));
document.querySelector("#app").addEventListener('contextmenu', function(event) {
    event.preventDefault();
    menu.popup(event.x, event.y);
    return false;
});
```

> Electron不支持`prompt`，可以使用www.npmjs.com/package/dialogs代替

# 拖拽文件以及定制界面
## 拖拽文件
使用了`HTML5`的`d&d`
```js
function stopDefaultEvent (event) {
	event.preventDefault();
	return false;	
}

window.ondragover = stopDefaultEvent;
window.ondrop = stopDefaultEvent;

function displayImageInIconSet (filePath) {
	var images = window.document.querySelectorAll('#icons img');
	for (var i=0;i<images.length;i++) {
		images[i].src = filePath;
	}
}

function displayIconsSet () {
	var iconsArea = window.document.querySelector('#icons');
	iconsArea.style.display = 'block';
}

function interceptDroppedFile () {
	var interceptArea = window.document.querySelector('#load-icon-holder');
	interceptArea.ondrop = function (event) {
		event.preventDefault();
		if (event.dataTransfer.files.length !== 1) {
			window.alert('You have dragged too many files into the app. Drag just 1 file');
		} else {
			interceptArea.style.display = 'none';
			displayIconsSet();
			var file = event.dataTransfer.files[0];
			displayImageInIconSet(file.path);
		}
		return false;
	};
}

window.onload = function () {
	interceptDroppedFile();
};
```

## 模拟操作系统原生样式
### 检测操作系统
```js
var os 		= require('os');
var platform 	= os.platform();


function addStylesheet (stylesheet) {
  var head = document.getElementsByTagName('head')[0];
  var link = document.createElement('link');
  link.setAttribute('rel','stylesheet');
  link.setAttribute('href',stylesheet+'.css');
  head.appendChild(link);
}

function labelOS (osName) {
  document.getElementById('os-label').innerText = osName;
}

function initialize () {
  switch (platform) {
  	case 'darwin':
  	  addStylesheet('mac');
      labelOS('macOS');
  	  break;
  	case 'linux':
  	  addStylesheet('linux');
      labelOS('Linux');
  	  break;
  	case 'win32':
  	  addStylesheet('windows');
      labelOS('Microsoft Windows');
  	  break;
  	default:
  	  console.log('Could not detect OS for platform',platform);
  }
}

window.onload = initialize;
```

`os.release()`可以进一步返回技术层面上的版本号（和产品显示的版本号可能不一致）

### 使用CSS匹配操作系统

* [Metro UI](https://metroui.org.ua)
* [Mac OS Lion CSS UI Kit](https://sakamies.github.io/Lion-CSS-UI-Kit)
* [Photon](http://photonkit.com)
* [Photon React](https://github.com/react-photonkit/react-photonkit)
* [React Desktop](http://reactdesktop.js.org)

# 网络摄像头
调用`HTML5`的API，不需要申请权限

`NW.js`使用隐藏的`<input type="file">`元素来弹出保存文件对话框
```js
const fs = require('fs');
let photoData;
let saveFile;
let video;

function bindSavingPhoto () {
    saveFile.addEventListener('change', function () {
        let filePath = this.value;
        fs.writeFile(filePath, photoData, 'base64', (err) => {
        if (err) alert('There was a problem saving the photo:', err.message);
        photoData = null;
        });
    });
}

function initialize () {
    saveFile = window.document.querySelector('#saveFile');
    video = window.document.querySelector('video');

    let errorCallback = (error) => {
        console.log('There was an error connecting to the video stream:', error);
    };

    window.navigator.webkitGetUserMedia({video: true}, (localMediaStream) => {
        video.src = window.URL.createObjectURL(localMediaStream);
        video.onloadedmetadata = bindSavingPhoto;
    }, errorCallback);
}

function takePhoto () {
    let canvas = window.document.querySelector('canvas');
    canvas.getContext('2d').drawImage(video, 0, 0, 800, 600);
    photoData = canvas.toDataURL('image/png').replace(/^data:image\/(png|jpg|jpeg);base64,/, '');
    saveFile.click();
}

window.onload = initialize;
```

`Electron`使用API
```js
const electron = require('electron');
const dialog = electron.remote.dialog;
const fs = require('fs');
let photoData;
let video;

function savePhoto (filePath) {
    if (filePath) {
        fs.writeFile(filePath, photoData, 'base64', (err) => {
        if (err) alert(`There was a problem saving the photo: ${err.message}`);
        photoData = null;
    });
  }
}

function initialize () {
    video = window.document.querySelector('video');
    let errorCallback = (error) => {
        console.log(`There was an error connecting to the video stream: ${error.message}`);
    };

    window.navigator.webkitGetUserMedia({video: true}, (localMediaStream) => {
        video.src = window.URL.createObjectURL(localMediaStream);
    }, errorCallback);
}

function takePhoto () {
    let canvas = window.document.querySelector('canvas');
    canvas.getContext('2d').drawImage(video, 0, 0, 800, 600);
    photoData = canvas.toDataURL('image/png').replace(/^data:image\/(png|jpg|jpeg);base64,/, '');
    dialog.showSaveDialog({
        title: "Save the photo",
        defaultPath: 'myfacebomb.png',
        buttonLabel: 'Save photo'
    }, savePhoto);
}

window.onload = initialize;
```

# 存储应用数据
名字 | 数据库类型 | 类型 | 网址 
- | :-: | :-: | :-:
IndexedDB | key/value | 浏览器API | https://is.gd/wwDSgj 
localStorage | key/value | 浏览器API | https://is.gd/3XbaFQ 
Lovefield | relationship | 3rd | https://github.com/google/lovefield
PouchDB | document | 3rd | https://pouchdb.com
SQLite | relationship | embeded | http://sqlite.com
NeDB | document | embeded | http://is.gd/f44eap
LevelDB | key/value | embeded | http://leveldb.org
Minimongo | document | 3rd | https://is.gd/yTRXhe

# 复制粘贴
## NW.js
`NW.js`只支持文本类型的剪贴板数据
```js
const gui = require('nw.gui');
const clipboard = gui.Clipboard.get();

clipboard.set('FooBar', 'text');
clipboard.get('text');
clipboard.clear();
```

## Electron
`Electron`能支持文本、图片、HTML、RTF类型的剪贴板数据
```js
const electron = require('electron');
const clipboard = electron.clipboard;

clipboard.writeText('FooBar');
clipboard.readText();
clipboard.clear();

clipboard.writeImage(image);
clipboard.readImage();
clipboard.writeRTF(richText);
clipboard.readRTF();
clipboard.writeHTML(html);
clipboard.readHTML();
```

# 键盘快捷键
[贪吃蛇](https://github.com/paulbjensen/cross-platform-desktop-applications/tree/master/chapter-14)

应用内快捷键使用事件监听`keydown`

`NW.js`实现全局键盘快捷键
```js
const pauseKeyOptions = {
    key: 'Ctrl+P',
    active: () => {
        alert("Bingo");
    },
    failed: () => {

    }
};
const pauseShortcut = new nw.Shortcut(pauseKeyOptions);
nw.App.registerGlobalHotKey(pauseShortcut);

process.on('exit', () => {
    nw.App.unregisterGlobalHotKey(pauseShortcut);
});
```

`Electron`实现全局键盘快捷键
```js
//main.js
const { globalShortcut } = electron;
app.on('ready', () => {
    mainWindow = new BrowserWindow();
    mainWindow.loadURL(`file://${__dirname}/index.html`);
    mainWindow.on("closed", () => {
        mainWindow = null;
    });
    const pauseKey = globalShortcut.register('CommandOrControl+P', () => {
        mainWindow.webContents.send('togglePauseState');
    });
});

app.on('will-quit', () => {
    globalShortcut.unregister('CommandOrControl+P');
});

//index.html
const ipcRenderer = require('electron').ipcRenderer;
ipcRenderer.on('togglePauseState', () => {
    alert('Bingo');
});
```

# 桌面通知
## Electron
安装`electron-notifications`(https://github.com/blainesch/electron-notifications)
```
npm install --save electron-notification
```

```js
const notifier = require('electron-notifications')

notifier.notify('Calendar', {
    message: 'Event begins in 10 minutes',
    icon: 'http://cl.ly/J49B/3951818241085781941.png',
    buttons: ['Dismiss', 'Snooze'],
});
```

![electron-notification](https://github.com/swordrain/electron-nwjs/blob/master/image/electron-notification.png)

## NW.js
使用`Chrome`原生API
```js
let notify = Notification;
new notify(`New Notification`, {
    body: 'Foobar',
    icon: 'youtube.png'
});
```
![nwjs-notification](https://github.com/swordrain/electron-nwjs/blob/master/image/nwjs-notification.png)

# 测试
* 使用`Mocha`进行单元测试
* 使用`Spectron`进行功能测试
* 使用`Cucumber`进行集成测试

# 调试与性能
## 调试
`NW.js`需要下载使用`SDK`版本才有调试工具支持
```
npm install nwjs --nwjs_build_type=sdk
node_modules/.bin/nw install 0.30.2-sdk
node_modules/nw/bin/nw
```

```json
{
    "window": {
        "toolbar": true
    }
}
```

在`Windows`环境下，点右键会有调试菜单

![nwjs-debug](https://github.com/swordrain/electron-nwjs/blob/master/image/nwjs-debug.png)

> 也可以使用 `--remote-debugging-port=PORT` 开启远程调试，然后使用`node debug -p PROCESS_ID`

`Electron`在默认的菜单里可以打开开发者工具，或者使用快捷键`Ctrl(Command) + Shift + I`

`Electron`也有自己的调试工具
```
npm i devtron --save-dev
```

然后在开发者工具中的`console`中执行
```js
require('devtron').install()
```

安装完成后，开发者工具里多一个`Devtron`选项卡(http://electron.atom.io/devtron)

![devtron](https://github.com/swordrain/electron-nwjs/blob/master/image/devtron.png)

## 性能
使用开发者工具

# 打包
## NW.js创建Windows的exe
1. 将代码压缩成zip文件`package.nw`
2. 将`package.nw`复制到和`nw.exe`在同一个文件夹中
3. 运行`copy /b nw.exe+package.nw lorikeet.exe`

## Electron创建Windows的exe
1. 安装`asar` `npm install -g asar`
2. `asar pack hello-world-electron app.asar`
3. 下载一份`electron`二进制文件并解压
4. 将`app.asar`复制到目录的`resources`文件夹下
5. 修改`exe`文件名称
6. 安装`rcedit` `npm install -g rcedit`
7. 修改图标`rcedit electron.exe --set-icon "my-app-icon.ico"`

或者使用`electron-packager`来打包(www.npmjs.com/package/electron-packager)

## NW.js创建Windows启动安装器
* Nullsoft的NSIS
* Inno Setup
* WinRAR

## Electron创建Windows启动安装器
* Grunt-Electron-Installer
* Electron-installer-squirrel-windows
* electron-packager
* electron-builder

## NW.js创建Mac OS可执行文件
```
npm install -g nw-builder
nwbuild lorikeet-nwjs -p osx64
```

如果要进一步转成`dmg`文件
```
npm install -g appdmg
appdmg <json-path> <dmg-path> # json文件里包含了appdmg的配置信息
```

## Electron创建Mac OS可执行文件
```
npm i electron-builder --save-dev
```

确保`package.json`里包含`name`、`version`、`author`、`description`字段，并添加
```json
{
    "build": {
        "mac": {
            "title": "Hello World",
            "icon": "icon.icns",
            "background": "background.png",
            "icon-size": 80,
            "contents": [{
                "x": 448,
                "y": 220,
                "type": "link",
                "path": "/Applications"
            }, {
                "x": 192,
                "y": 220,
                "type": "file",
                "path": "dist/hello-world-darwin-x64/hello-world.app"
            }]
        }
    }
}
```

执行`npm run pack && npm run dist`

## NW.js为Linux创建可执行应用
```
npm install -g nw-builder
nwbuilder cirrus -p linux32,linux64
```

## Electron为Linux创建可执行应用
* Grunt-build-atom-shell
* electron-packager
* electron-builder

