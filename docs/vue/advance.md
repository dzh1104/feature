## 兼容问题

- 引入**babel-polyfill**
- 对于低版本的IE浏览器，可以用一个提示页，引导用户下载高级浏览器(在index.html中去添加相关提示信息和浏览器下载地址)

## 响应式原理

> 如何追踪变化

一个普通的 JavaScript 对象传给 Vue 实例的 `data` 选项，Vue 将`遍历`此对象所有的属性，并使用 `Object.defineProperty` 把这些属性全部转为 getter/setter，在内部它们让Vue追踪依赖，在属性被访问和修改时通知变化

每个组件实例都有相应的 `watcher` 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 `setter` 被调用时，会通知 `watcher` 重新计算，从而致使它关联的组件得以更新

> 检测变化的注意事项

受现代 JavaScript 的限制 (以及废弃 `Object.observe`)，Vue 不能检测到对象属性的添加或删除。由于 Vue 会在初始化实例时对属性执行 `getter/setter` 转化过程，所以属性必须在 data 对象上存在才能让 Vue 转换它，这样才能让它是响应的

```js
// 单个属性
Vue.set(vm.userProfile, 'age', 27) 

// 多个属性
this.userProfile = Object.assign({}, this.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

> 声明响应式属性

由于 Vue 不允许`动态`添加根级响应式属性，所以你必须在初始化实例前声明根级响应式属性，哪怕只是一个空值

> 异步更新队列

Vue 异步执行 `DOM` 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。

Vue 在内部尝试对异步队列使用原生的 `Promise.then` 和 `MessageChannel`，如果执行环境不支持，会采用 `setTimeout(fn, 0)` 代替。

为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用 `Vue.nextTick(callback)`

> 列表渲染的一些问题

由于 JavaScript 的限制，Vue 不能`检测`以下`变动`的数组

- 利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
- 修改数组的长度时，例如：`vm.items.length = newLength`

解决第一类问题
- Vue.set(example1.items, indexOfItem, newValue)
- example1.items.splice(indexOfItem, 1, newValue)

解决第二类问题
example1.items.splice(newLength)
