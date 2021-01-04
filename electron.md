# Debug

## 主进程调试

* vscode调试
* chrome调试

## 渲染进程调试

* 打开开发者工具调试

# 创建窗口

```javascript
const mainWindow=new BrowserWindow({
  width:800,
  height:600,
  webPreferences:{
    preload:path.join(__dirname,'preload.js')//预加载
    nodeIntegration:true //开启渲染进程的node模块
  }
  
})
```

# html5拖拽事件

```javascript
const dragWrapper=document.querySelector('.drag_test')//拖拽释放区域
dragWrapper.addEventListener('drop',e=>{
    e.preventDefault() //阻止默认事件
    const files=e.dataTransfer.files//倘若拖拽的是本地文件
    if(files&&files.length>0){
        const path=files[0].path
        const content=fs.readFileSync(path)
        console.log(content.toString())

    }
})
dragWrapper.addEventListener('dragover',e=>{
    e.preventDefault() // dragover事件也要阻止默认事件
})
```



# api

> app.on('name',()=>{})

## app

```javascript
import {app} from 'electron'
```

### ready事件

> 当electron完成初始化时被触发

### window-all-closed事件

> 所有窗口被关闭时触发

### before-quit事件

> 在应用程序开始关闭窗口之前触发

### will-quit事件

> 当所有窗口已关闭且应用程序将退出时触发

### quit事件

> 程序退出时触发

## webContents

> 是一个事件发出者

### did-finish-load事件

> 导航完成时触发

### dom-ready事件

> dom挂载完成触发

### send方法

```javascript
window.webContents.send('eventName',message)// 发送给渲染进程的消息
```

## --node模块--process

### arch属性

>  获取操作系统信息x64或者x32

### platform

> 获取用户操作系统信息linux、windows...

## window

### open('url',标题名)

> 打开子窗口并加载一个url，返回一个BrowserWindowProxy类型

### opener.postMessage(message)

> 传递消息给父窗口，父窗口监听message事件接收

## BrowserWindow

> new BrowserWindow(**属性设置**)

### ready-to-show事件

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow({show:false}) // 先不显示
win.once('ready-to-show',()=>{
  win.show()
})
```

## globalShortcut

> 设定快捷键

## ipcMain主进程，ipcRenderer渲染进程

### on('eventName',message)

> 监听事件

### send('eventName',message)

> 发送事件

### event.reply('eventName',message)

> 针对某个事件进行回复



功能上：

1.通过调用多平台开源api，整合多平台资源于一个客户端内方便使用

2.支持用户登陆注册并记录用户习惯实现定向推荐

3.支持本地歌曲播放并联网匹配歌词、歌手等信息

4.通过用户习惯生成偏好分析图表

5.可根据音频生成可视化旋律效果或调节音调等

技术上：

1.使用react、redux搭建前端，使用electron生成客户端构建多平台应用

2.使用nodejs搭建后端，设计restful API

3.使用mongodb数据库进行数据存储

4.使用JWT认证

