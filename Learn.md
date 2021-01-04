# Recommend Translation

## CSS

### 画三角

用正方形旋转，两条边透明实现

```css
width: 7px;
height: 7px;
transform: rotateZ(45deg)
border-top: 1px solid red;
border-left: 1px solid red;
background-color: white;
```

## JS

### es6解构赋值

```javascript
const {threadID:currentThreadId}=getState() // currentThreadId为新定义的变量
```

### react动画

* 通过设置state，css设置transition，当需要删除该dom的时候，先await动画效果并加上class，再实际删除dom，从而起到渐出的效果

### 组件抽离

* 对于需要拥有自己生命周期的组件，需要单独抽离实现，高内聚低耦合。
* 代码复用不是问题，主要是逻辑需要分离，使整体代码逻辑清晰不混杂。

### css效果

* 对于对话框中的小三角，需要注意在鼠标上方或者下方时的变化，通过判断条件动态添加class实现样式的变化

  ```css
  .up {
    width: 7px;
    height: 7px;
    transform: rotateZ(45deg)
    border-top: 1px solid red;
    border-left: 1px solid red;
    background-color: white;
  }
  .up.down {
    transform: rotateZ(-135deg)
  }
  // react
  import s from './xx.module.less'
  isUp : Boolean
  <App className={classnames(s.up,isUp && s.down)}/>
  ```

  

## redux

### 结构

* 在action文件内写好type和payload类型
* 在reducer文件中写好state的type，在init函数中写好初始值
* 在switch中写case处理action，**不能直接修改state**。若使用immer包，则draftstate相当于原state的深拷贝，因此可以直接在draftstate上更改。

* 数据，函数通过connector透传入组件

## TS

### 类型注释

* 注释需要清晰，每个参数写清类型。

  ```typescript
  function xxx(this: ThreadModule,threadId: string){...} //this需要作为第一个参数，声明该函数需要绑定的this
  // 使用时：
  xxx.call(ThreadModule,'123')
  // 传递时
  @connect(xxx: xxx.bind(ThreadModule))
  
  ```



# Auto Refresh

## JS

### 拉取sdk

```cmd
cd app
yarn download sdk_[commitId]
```

### debug

* 对于根据监听事件变化而触发的事件，可以通过settimeout延迟触发以观察变化

### 监听事件

* 监听rust sdk发出的事件，通过redux响应式更新页面。
  * command在sdk文件夹中的cmd-info.ts中，需要在pushCmd.ts文件中引用指令
  * 在Module.ts文件中注册监听事件，返回类型可以从pb中获取
  * 通过dispatch改变store中的state
  * 将state透传至需要该state的地方

## CSS

### 动态省略号

```css
<span class="dots">...</span>
.dots{
  width:0.75rem;
  overflow:hidden;
  animation: dot 3s infinite step-start;
}
@keyframes dot {
  25%{width:0;margin-right:0.75rem;}
  50%{width:0.25rem;margin-right:0.5rem}
  75%{width:0.5rem;margin-right:0.25rem}
  100%{width:0.75rem;margin-right:0}
}

```

# select all 二期优化

## TS

* 使用ref时，传入泛型类型

  ```typescript
  const progressBarRef=useRef<HTMLDivELement>(null)
  useEffect(()=>{
    const progressBarDom=progressBarRef.current
  },[])
  ```

* 多个异步请求时，需要注意请求发出到返回期间可能出现的操作。

  * 例如：监听进度条的轮询间隔时间为2s，在最后完成的时候只用了1s，但轮询2s导致完成时间的延迟，因此需要再额外监听完成的push以保证准确的完成进度。

* 要求处理时间5s内展示一个组件，5s后切换为另一个组件，可行的方法：

  * 使用5s的定时器、promise和互斥锁实现

    ```typescript
    //...
    let mutex = true
    Promise.all(fuc:Promise<number>).then(()=>{
      if(mutex){
        mutex = false
        //...组件1
      }
      
    })
    setTimeout(()=>{
      if(mutex) {
        mutex = false
        //...组件2
      }
    },5000)
    ```

* 捞log:Library/Application Support/Lark/sdk_storage

