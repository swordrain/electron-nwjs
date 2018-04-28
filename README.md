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

![nw.js](https://github.com/swordrain/electron-nsjw/blob/master/image/nwjs-win.png)

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

![nwjs-helloworld](https://github.com/swordrain/electron-nsjw/blob/master/image/nwjs-helloworld.png)

`nw.js`特性

* 一套访问操作系统的API
* 使用`Node.js`
* 为不同的操作系统构建可执行文件

