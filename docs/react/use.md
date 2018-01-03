## use

##jsx

一种 JavaScript 的语法扩展，完全是在 JavaScript 内部实现的

- 使用三目运算符代替if/else，语句替换成表达式
- 列表循环需要加key，而且key不能为''等

##路由和菜单

> react-router@4不再支持路由嵌套，无法在一个统一的文件中生成所有路由结构

> exact属性是react-router@4新增的属性，在router.js中路由配置不能添加exact

> 路由配置的顺序也会影响渲染问题

```jsx
<Switch>
  <Route path="/user" render={props => <UserLayout {...props} />} />
  <Route path="/" render={props => <BasicLayout {...props} />} />
</Switch>
```

首先这是router.js中的路由配置不能添加exact，如果将path="/"放在前面，当路径为**/user**时，由于没有使用**exact**属性 + 使用了**Switch**，所以只会渲染path="/"的组件




