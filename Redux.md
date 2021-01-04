# Redux学习笔记

## 基本概念

* Store

  ```javascript
  import {createStore} from 'redux';
  const {dispatch,getState,subscribe}=createStore(reducer);
  // getState()获取状态
  // dispatch(Action) dispatch接收一个Action对象作为参数并发送，该方法会触发reducer的自动执行，因此在createStore时需要传入reducer函数
  //subscribe方法设置监听函数，若state发生变化，就自动执行这个函数，该方法返回一个函数，调用该函数则解除监听
  ```

* Action

  > Action是一个对象，其中的`type`属性是必须的，表示Action的名称
  >
  > Action描述当前发生的事情，是改变State的唯一方法

  ```javascript
  const action = {
    type: 'ADD_TODO' // type属性是必须的
    payload:'+1'
  }
  ```

* Reducer

  > Reducer是一个函数，接受Action和当前state作为参数并返回新的State。
  >
  > Store收到Action之后，必须给出新的State，这样View才会变化，这个计算过程就是Reducer

  ```javascript
  const reducer=function (state,action){
    //,,,
    return new_state
  }
  ```

  

## Store方法

### dispatch(Action)

> 是View发出Action的唯一方法，会自动触发Reducer的自动执行，因此在createStore时需要接受Reducer作为参数

```javascript
store.dispatch(Action)
```

### subscribe(listen)

> 监听传入的函数，一旦state发生变化，就自动执行该函数，因此只要把View的更新函数(render方法或者setState方法)放入，就会实现View的自动渲染
>
> 方法返回一个函数，调用该函数解除监听

### getState()

> 获取当前State

### createStore(reducer,defaultState,applyMiddleware(middleware))

> 初始化Store，第一个参数传入Reducer，第二个可选参数表述State的最初状态,第三个可选参数是插入中间件方法

```javascript
import createLogger from 'redux-logger'
const logger=createLogger()
const store=createStore(
  reducer,
  defaultState,
  applyMiddleware(logger)
)
```

# react-redux

> 容器组件与展示组件相分离开发

|                | 展示组件                   | 容器组件                           |
| -------------: | :------------------------- | ---------------------------------- |
|           作用 | 描述如何展现（骨架、样式） | 描述如何运行（数据获取、状态更新） |
| 直接使用 Redux | 否                         | 是                                 |
|       数据来源 | props                      | 监听 Redux state                   |
|       数据修改 | 从 props 调用回调函数      | 向 Redux 派发 actions              |
|       调用方式 | 手动                       | 通常由 React Redux 生成            |

## 展示组件

* 仅描述如何展现，数据由props获得

  ```javascript
  const TodoList = ({todos,onTodoClick})=>{
    console.log(todos)
    return (
    	<button onClick={()=>onTodoClick(todos.id)} />
    )
  }
  ```

  

## 容器组件

* 负责连接展示组件到Redux，核心是使用connect(mapStateToProps(),mapDispatchToProps())(UI)实现

## connect()()

* mapStateToProps(state,ownProps)

  > 通过该函数指定如何把当前state映射到**展示组件**的props中,返回对象类型 `ownProps`表示组件本身的props

  ```javascript
  const mapStateToProps=state=>{
    return {
      todos:state.todos    
      //此时展示组件中props.todos就是state.todos
    }
  }
  ```

* mapDispatchToProps(dispatch,ownProps)

  > 该方法可以分发action，通过接收`dispatch()`方法并返回期望注入到展示组件的props中的回调方法

  ```javascript
  const mapDispatchToProps=dispatch=>{
    return {
      onTodoClick:id=>{
        dispatch({type:id})
      }
      //此时展示组件中的onTodoClick方法就是id=>{dispatch(Action)}
    }
  }
  ```

* connect()()

  > connect()()用于连接容器组件和展示组件并返回一个组件，本质上是一个高阶函数，返回一个HOC

  ```javascript
  import {connect} from 'react-redux'
  const newItem=connect(mapStateToProps,mapDispatchToProps)(TodoList)
  export default newItem
  
  //es6装饰器模式
  @connect(mapStateToProps,mapDispatchToProps)
  export default class TodoList extends React.Component{
    //...
  }
  
  //装饰器
  function HocUITest(name){
    return function ComponentWrapper(Component){
      return class WrapComponent extends React.Component{
        //...
      }
    }
  }
  // 简写成：
  @HocUITest('张三')
  class Component //...
  ```

## 使用

```javascript
import newItem from './newItem'
const App=()=>(
  <div>
		<newItem /> //名字是接收connect返回值的变量名
  </div>
)
export default App
```

## 传入Store

### Provider组件

> 只需要在渲染根组件的时候使用即可让所有容器组件访问store

```javascript
let store=createStore(Reducer)
render(
	<Provider store={store}>
  	<App/>
  </Provider>,
	document.getElementById('root')
)
```

