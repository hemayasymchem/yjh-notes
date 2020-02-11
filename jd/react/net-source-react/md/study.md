### 项目中使用到的npm包

    1 react-hot-loader
    2 query-string (我觉得应该是和qs差不多吧)
    3 js-cookie (设置cookie的)
    4 connected-react-router
      是一个绑定react-router到redux的组件，来实现双向绑定router的数据到redux store中
      可以在action中实现对路由的操作
      项目中使用到的 connectRouter routerMiddleWare
    5 history 历史记录库 用到的 createBrowserHistory
    6 redux-thunk 用到的是thunk 因为redux store中仅仅支持同步数据流 使用thunk等中间件 可以实现异步
    7 react-lazy-load 图片懒加载

### 关于React的
    1 componentDidCatch React内置函数 如果render函数抛出异常 就会触发这个函数   
    2 { PureComponent} React中的



### rudux 
    1 applyMiddleWare
    2 compose
    3 window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ (这句代码还不知道什么意思 应该是调试工具之类的)

###  Router
    1 react-helmet 使用到了 Helmet React文档头管理器
    2 react-loadable 代码分割 并提供loader时候的组件

### 其他的npm包

    1 normalize 
    2 rmada
      rmada 中的 { omit path clone }
    3 @jdcfe/yep-react 京东自己的移动端包



### 其他

     1 webpack 热加载问题
       代码
       if (module.hot) {
           module.hot.accept('./reducer', () => {
               store.replaceReducer(connectRouter(history)(rootReducer))
           })
       }

### history插件和 connected-react-router插件的组合使用
```
import {createBrowserHistory} from 'history'
export default createBrowserHistory({
    basename: process.env.NODE_ENV === 'production' ? '/s' : '
})


import { applyMiddleware, compose, createStore } from 'redux';
import { connectRouter, routerMiddleware } from 'connected-react-router';
import rootReducer from './reducers';
import history from './history';
import api from '@/store/middlewares/api';
import thunk from 'redux-thunk';
const composeEnhancer = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
  connectRouter(history)(rootReducer), // new root reducer with router state
  //{},
  composeEnhancer(
    applyMiddleware(
      thunk,
      api,
      routerMiddleware(history) // for dispatching history actions
    )
  )
);

// Hot reloading
if (module.hot) {
  // Reload reducers
  module.hot.accept('./reducers', () => {
    store.replaceReducer(connectRouter(history)(rootReducer));
  });
}

export default store;
```       

[csdn-React-history参考资料1](https://blog.csdn.net/mcYang0929/article/details/89315117)
```
在react-router中可以使用 <Link> 但是在组件外部使用
import { createBrowserHistory } from 'history'
const history = createBrowserHistory({
    basename: '',
    forceRefresh: true
})
export default history
在其他位置
import history form 'xxx/history'
history.push()
  history.push('/a')
  history.push('/a', {name: ''})
  history.push({
      pathname: '/a',
      state: {
          name: '
      }
  })
replace 方法 和 push方法使用形式一致 replace的作用是取代当前历史记录
go 前进或者后退 history.go(-1)
goBack goFroward

```

[另外一个文献history](https://www.cnblogs.com/ye-hcj/p/7741742.html)
```
history的三种创建方式
  createBrowserHistory({  // 现在浏览器使用
    basename： '', 基础链接
    forceRefresh: false, 是否强制刷新整个页面
    keyLength: 6, location.key的长度
    xxx
  })
  未完待续
```