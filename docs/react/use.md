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

## 组件渲染

当React遇到的元素时用户自定义的组件，它会将JSX属性作为单个对象传递给该组件，这个对象称之为"props"

> 如果是函数组件，props对象将作为函数参数
```jsx
const TestComponent = (props) => (
  <div>Hello, {props.name}</div>
);
```

> 如果是类组件，props对象将作为类的属性
```jsx
class TestComponent extends Component {
  constructor(props) {

  }
  render() {
    return (
      <div>Hello, {this.props.name}</div>
    );
  }
}
```

## JSX属性传递的方式

**key=value**或者用对象扩展符**...Object**
```jsx
const TestComponent = (props) => (
  <div>Hello, {props.name}</div>
);
<TestComponent name="dzh" />
<TestComponent {...{name: 'dzh'}} />
<TestComponent {...{...{name: 'dzh'}, ...{age: 18}} />
```

## props

React严格的规则: props是只读的

## state

state与props十分相似，但是状态是私有的，完全受控于当前组件

## 事件处理

事件传递参数

```js
import React from 'react';

class Popper extends React.Component {
  preventPopByBind(name, e) {
    // bind 事件对象e要放在最后
    console.log('e', e);
    e.preventDefault();
    alert(name);
  }

  preventPopByArrow(name, e) {
    // arrow 事件对象 和 对应的事件处理函数的参数顺序对应即可
    console.log('e', e);
    e.preventDefault();
    alert(name);
  }

  render() {
    return (
      <div>
        <p>hello</p>
        {/* Pass params via arrow method. */}
        <a
          href="https://reactjs.org"
          onClick={(e) => this.preventPopByArrow('arrow', e)}
        >
          Click by Function.prototype.bind
        </a>
        <br />
        {/* Pass params via bind() method. */}
        <a
          href="https://reactjs.org"
          onClick={this.preventPopByBind.bind(this, 'bind')}
        >
          Click by Arrow Function
        </a>
      </div>
    );
  }
}

export default Popper;
```

## 类组件与函数组件

类组件特有的: 
- state
- 生命周期
- ref(不可再函数式组件上使用，但是可以在函数式组件内部使用，只要它指向一个 DOM 元素或者 class 组件)

## 组件名

- 大写开头的 JSX 标签表示一个 React 组件
- JSX 中的点表示法来引用 React 组件(这个不要求必须大写)

## 在运行时选择类型

你不能使用表达式来作为 React 元素的标签

属性值条件渲染组件

动态组件 将类型赋值给大写开头的变量



## 常用第三方

- nprogress 路由进度条
- react-loadable 路由懒加载