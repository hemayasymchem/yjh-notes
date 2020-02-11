[官网地址](https://www.npmjs.com/package/connected-react-router)
```
目前最新的版本是 v5.0.0
使用步骤 (代码抄一遍)

1 reducers.js

  import { combineReducers } from 'redux'
  import { connectRouter } from 'connected-react-router'

  const createRootReducer = (history) => combineReducers({
      router: connectRouter(history), // 第一个参数必须是这个
      ...reducers // rest of your reducers 
  })
  export default createRootReducers

2 create a redux store
  configureStore.js
  import { createBrowserHistory } from 'history'
  import { applyMiddleware, compose, createStore } from 'redex'
  import { routerMiddleware  } from 'connected-react-router'
  import createRootReducers from './reducers.js'

  export const history = createBrowserHistoryy()
  export default function configureStore(preloadState) {
      const store = createStore(
          createRootReducers(history),
          preloadState,
          compose(
              applyMiddleware(
                  routerMiddleware(history)
                  ...other middleware // 其他中间件
              )
          )
      )
      return store
  }

  3 router.js

     import { Provider } from 'react-redux'
     import { Route, Switch } from 'react-router-dom'
     import { ConnectedRouter } from 'connected-react-router'
     import configureStore, { history } from './configureStore'

     const store = configureStore(// initial state)

     ReactDOM.render(
         <Provider store={store}>
            <ConnectedRouter history={history}>
               <>
                 <Switch>
                   <Route exact path="/" component={Home}></Route>
                   ...
                 </Switch>
               </Route>
            </ConnectedRouter>
         </Provider>,
         document.getElementById('root')
     ) 

    
```