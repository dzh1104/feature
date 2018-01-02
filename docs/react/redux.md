## react-redux

当 store 发生变化，React 组件会手动重新渲染。在实际的项目中，可以使用 React 和 Redux 已绑定、且更高效的 [React Redux](https://github.com/reactjs/react-redux)

深入理解在 Redux 中 state 的更新与组件是如何共同运作， reducer 如何委派 action 给其它 reducer，如何使用 React Redux 从展示组件中生成容器组件

如何使用 [Redux Undo](https://github.com/omnidan/redux-undo) 打包 reducer，仅增加几行代码实现撤销/重做功能

## action

Action 是把数据从应用（之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的唯一来源。一般来说你会通过 `store.dispatch()` 将 action 传到 store

**尽量减少在 action 中传递的数据**

## Action 创建函数

Action 创建函数 就是生成 action 的方法

~~~js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
~~~

被绑定的 action 创建函数 来自动 dispatch

~~~js
const boundAddTodo = (text) => dispatch(addTodo(text))
const boundCompleteTodo = (index) => dispatch(completeTodo(index))
~~~

store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 `react-redux` 提供的 `connect()` 帮助器来调用。bindActionCreators() 可以自动把多个 action 创建函数 绑定到 dispatch() 方法上

## 设计 State 结构

在 Redux 应用中，所有的 state 都被保存在一个单一对象中

## Reducer

reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state

编写 reducer，并让它来处理之前定义过的 action

 ES6 参数默认值语法 来精简代码，指定 state 的初始状态

 ~~~js
 function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
~~~

Object.assign() 对象展开运算符

## combineReducers()

Redux 提供了 combineReducers() 工具类来做reducer 合成

combineReducers() 所做的只是生成一个函数，这个函数来调用你的一系列 reducer，每个 reducer 根据它们的 key 来筛选出 state 中的一部分数据并处理，然后这个生成的函数再将所有 reducer 的结果合并成一个大的对象。没有任何魔法。正如其他 reducers，如果 combineReducers() 中包含的所有 reducers 都没有更改 state，那么也就不会创建一个新的对象。

## Store

创建 Redux store。store 能维持应用的 state，并在当你发起 action 的时候调用 reducer

Redux 应用只有一个单一的 store。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store

使用 combineReducers() 将多个 reducer 合并成为一个。现在我们将其导入，并传递 createStore()

Redux store 保存了根 reducer 返回的完整 state 树

这个新的树就是应用的下一个 state！所有订阅 store.subscribe(listener) 的监听器都将被调用；监听器里可以调用 store.getState() 获得当前 state。

现在，可以应用新的 state 来更新 UI。如果你使用了 `React Redux` 这类的绑定库，这时就应该调用 component.setState(newState) 来更新。

~~~js
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
~~~

createStore() 的第二个参数是可选的, 用于设置 state 初始状态。这对开发同构应用时非常有用，服务器端 redux 应用的 state 结构可以与客户端保持一致, 那么客户端可以将从网络接收到的服务端 state 直接用于本地数据初始化。

~~~js
let store = createStore(todoApp, window.STATE_FROM_SERVER)
~~~

## subscribe

~~~js
// 每次 state 更新时，打印日志
// 注意 subscribe() 返回一个函数用来注销监听器
const unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)

// 停止监听 state 更新
unsubscribe();
~~~

## 容器组件和展示组件

大部分的组件都应该是展示型的，但一般需要少数的几个容器组件把它们和 Redux store 连接起来

不要手写容器组件，而使用 React Redux 的 connect() 方法来生成