###  store 创建action
```
  // actions 
  /**
  * @return fun 
  * 两个参数顾名思义 第一个参数就是 最后action里面的 type 第二个是 参数名字的集合（key)
  */
  function createActionCreator(type, ...argnNames) {
      return function(...args) {
          let action = { type }
          argNames.forEach((arg, index) => {
              action[argNams][index] = args[index]
              这里也可以换成 action[arg] = args[index]
          })
          return action
      }
  }

// 导出三个方法 都是需要一个参数啊
export const setDoctorList = createActionCreator("SET_DOCTOR_LIST", "payload");
export const setDoctorsById = createActionCreator("SET_DOCTORS_BY_ID", "payload");
export const deleteDoctorById = createActionCreator("DELETE_DOCTOR_BY_ID", "id")


```


### 官网 使用 applyMiddleWare的demo
```
  function DemoMiddleCom = ({getState}) => { // 参数有两个 getState 和 dispatch
      return (next) => (action) => { // action  就是dispatch的时候 的 action
          console.log('not dispatch')
          var res = next(action) // next(action) 才会执行你实际的dispatch 而且没有这句代码会报错
          console.log('dispatch has done')
          return res // 这个值 貌似不返回也不会报错啊
      }
  }

  const stroe = createStore(redecuers, applyMiddleWare(DemoMiddleCom))
```

### createStore 的参数
[简书上一个applyMiddleWare的用法介绍](https://www.jianshu.com/p/caff049d576c)
```
一般的使用方法
const store = createStore(reducer, applyMiddleWare(thunk))
使用默认参数
const store = createStore(reducer, defaultState, applyMiddleWare(thunk))
   参数 reducer会默认为 defaultState的值

这里的defaultState如果你传入参数的话 比如 { a: 1} 也会在store中 比你的reducer传入的值还要 靠前
```
[csdnRedux-crateStore用法介绍]
```
createStore(rudecer, [initState], enhancer) 创建一个store 一个应用中只能有一个store
参数
  reducer 一个纯函数 两个参数 state  和 action
  [initState](any) 初始时的state 
  enhancer(Function) 是一个组和 store creator的高阶函数 返回一个新的强化后的 store creator 与 middleWare类似

  redux源码中 如果有 enhancer的话
    return enhancer(createStore)(reducer, preloadState)

```


[简书-applyMiddleWare()用法的讲解](https://www.jianshu.com/p/caff049d576c)
```
  使用 applyMiddleWare(...middleWares)
  
  applyMiddleWare是使用包含自定义功能的middleware来扩展Redux的一个方法 
    主要是让你包装store的dispatch方法 来达到你的目的 并且可以链式调用

  例如：redex-thunk 支持 dispatch function ,以此让 action creator控制反转 被dispatch的funcion 会接受 dispatch作为参数 并可以异步调用
  middleware的签名函数 ({getState, dispatch}) => next => action 返回是一个 store enhancer  

  如果使用了applyMiddleWare 那么创建 store就有两种写法
  let store = createStore(reducer, preloadState, applyMiddleware(...middleware))

    或者
  let store = applyMiddle(...middleware)(createStore)(reducer, preloadState)

```
[csdn compose 和 reduce 方法](https://blog.csdn.net/astonishqft/article/details/82791622)

