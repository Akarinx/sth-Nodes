# React 路由

> react万物届组件

## 引入

```javascript
import {BrownserRouter as Router,Route,Link} from 'react-router-dom'
```

## 基本使用

> 基本的路由跳转和动态路由

```javascript
return (
	<Router>
  	<Switch>
  		<Route path="/" exact component={Index} /> // 一个路由页面
			<Route path="/:id" component={Second} /> // 带动态传值的路由 通过 state.match.params.id获取
  	</Switch>
  </Router>
)
```

## 重定向

* 函数式重定向：

  ```javascript
  constructor(prop){
    this.props.history.push("/home/")
  }
  ```

* 标签式:

  ```javascript
  import {Redirect} from 'react-router-dom'
  render(){
    return (
    	<Redirect to="/Home"
    )
  }
  ```


# config基本配置

```javascript
yarn add react-router-config
const routes = [
  { path: '/', component: Home, exact: true },
  { path: '/profile', component: Profile },
  { path: '/detail/:id', component: Detail },
]
// app.js
import { renderRoutes } from 'react-router-config'
import routes from './router'
    render() {
        // ...
        { renderRoutes(routes) }
    }
```

# 路由鉴权

```typescript
import { HashRouter as Router, Switch, Route, Redirect } from 'react-router-dom'
import router from './routerConfig'

export consgt AppRouter:React.FC = () => {
  const token = state.token //是否有token
  return (
  <Router>
  	<Switch>
    	{
    		router.map((item,index)=>{
          return (
          	 <Route key={index} path={item.path} exact render={props => // render会渲染加载页面
               (
                  !item.auth ? (<item.component {...props} />) : (token ? <item.component {...props} /> :
                   <Redirect to={{
                      pathname: '/',
                      state: { from: props.location }
                    }} />
                )
               )} />
        })
  		}
    </Switch>  
  </Router>
	)
}

// routerConfig
const routerConfig = [
  {
    path:'/',
    component:lazy(()=>import('@/components/App')), // 懒加载，需要与Suspence配合。
    auth:true // 标识是否需要鉴权
  }
]
```

