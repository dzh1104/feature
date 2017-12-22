## JavsScript表达式

可以与`其他类型的子代`混合使用，这通常对于`字符串模板`非常有用

~~~
function Hello(props) {
  return <div>Hello {props.addressee}!</div>;
}
~~~

## 渲染多个元素

一个 React 组件不能返回多个 React 元素，但是单个 JSX 表达式可以有多个子元素，因此，如果你希望一个组件渲染多个元素，你可以用 `<div>` 将其包起来 

~~~
<div>
  Here is a list:
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
</div>
~~~

React 组件也可以通过数组的形式返回多个元素

~~~
render() {
  // 不需要使用额外的元素包裹数组中的元素
  return [
    // 不要忘记 key :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
~~~

## 函数

通常情况下，插入 JSX 中的 JavsScript 表达式将被认作字符串、React 元素或这些内容的列表。然而，`props.children` 可以像其它属性一样传递任何数据，而不仅仅是 React 元素。例如，如果你使用自定义组件，则可以将调用 `props.children` 来获得传递的子代

~~~
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
~~~

>传递给自定义组件的子代可以是任何元素，只要该组件在 React 渲染前将其转换成 React 能够理解的东西

## “falsy” 值

JavaScript 中的一些 `“falsy”` 值(比如数字`0`)，它们依然会被渲染

下面的代码不会像你预期的那样运行，因为当 `props.message` 为空数组时，它会打印`0`

~~~
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
~~~

>要解决这个问题，请确保 && 前面的表达式`始终`为布尔值

~~~
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
~~~

相反，如果你想让类似 false、true、null 或 undefined 出现在输出中，你`必须`先把它转换成`字符串`

~~~
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
~~~

## PropTypes 类型检查

>类型检查发生在 defaultProps 赋值之后，所以类型检查也会应用在 defaultProps 上面

## Refs & DOM

>在典型的 React 数据流中, `属性（props）`是父组件与子代交互的唯一方式

要修改子组件，你需要通用新的 props 重新渲染它。但是，某些情况下你需要在典型数据流外强制修改子代。要修改的子代可以是 React 组件实例，也可以是 DOM 元素。对于这两种情况，React 提供了解决办法。

>`为DOM元素添加Ref`

~~~
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.focus = this.focus.bind(this);
  }

  focus() {
    // 直接使用原生 API 使 text 输入框获得焦点
    this.textInput.focus();
  }

  render() {
    // 使用 `ref` 的回调将 text 输入框的 DOM 节点存储到 React 
    // 实例上（比如 this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={(input) => { this.textInput = input; }} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focus}
        />
      </div>
    );
  }
}
~~~

>React 组件在加载时将 DOM 元素传入 ref 的回调函数，在卸载时则会传入 null。ref 回调会在componentDidMount 或 componentDidUpdate 这些生命周期回调`之前执行`

在类上设置一个属性使用 ref 回调是访问 DOM 元素的常见模式。首先的方法就如上面的例子中一样设置 ref。甚至还有更简短的写法： `ref={input => this.textInput = input}`

>`为类组件添加 Ref`

~~~
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
~~~

>需要注意的是，这种方法仅对 `class` 声明的 CustomTextInput 有效

>`函数式组件内部使用 ref`

~~~js
import React, {Component} from 'react';

function FunctionalComponentRef() {
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input 
        type="text"
        ref={input => textInput = input}
      />
      <input 
        type="button"
        onClick={handleClick}
        value="Click Focus"
      />
    </div>
  );
}

export default FunctionalComponentRef;
~~~

>`对父组件暴露 DOM 节点`

在极少数情况下，你可能希望从父组件访问子节点的 DOM 节点。通常不建议这样做，因为它会破坏组件的封装，但偶尔也可用于触发焦点或测量子 DOM 节点的大小或位置

虽然你可以向子组件添加 ref,但这不是一个理想的解决方案，因为你只能获取组件实例而不是 DOM 节点。并且，它还在函数式组件上无效

相反，在这种情况下，我们建议在子节点上暴露一个特殊的属性。子节点将会获得一个函数属性，并将其作为 ref 属性附加到 DOM 节点。这允许父代通过中间件将 ref 回调给子代的 DOM 节点

>适用于类组件和函数式组件

~~~js
import React, {Component} from 'react';

function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class ParentComponentUseDom extends Component {
  handleClick= () => {
    console.log(`input元素: ${this.inputElement}`);
  }
  
  render() {
    return (
      <div>
        <CustomTextInput 
          inputRef={el => this.inputElement = el}
        />
        <button onClick={this.handleClick}>点击显示input元素</button>
      </div>
    );
  }
}

export default ParentComponentUseDom;
~~~

## 提高性能shouldComponentUpdate

>`避免重复渲染`，通过重写这个`生命周期函数shouldComponentUpdate`来提升速度， 它是在`重新渲染过程`开始前触发的。 这个函数默认返回true，可使React执行更新

~~~js
shouldComponentUpdate(nextProps, nextState) {
  return true;
}
~~~

如果你知道在某些情况下你的组件不需要更新，你可以在shouldComponentUpdate内返回false来跳过整个渲染进程，该进程包括了对该组件和之后的内容调用render()指令

`demo`

~~~js
import React, {Component} from 'react';

class ShouldComponentUpdateUse extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 1
    };
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (this.props.color !== nextProps.color) {
      return true;
    }

    // if (this.state.count !== nextState.count) {
    //   return true;
    // }

    return false;
  }

  render() {
    return (
      <button 
        color={this.props.color}
        onClick={() => this.setState(prevState => ({count: prevState.count + 1}))}
      >
        {this.state.count}
      </button>
    );
  }
}

export default ShouldComponentUpdateUse;
~~~

## React.PureComponent

React提供了一个辅助对象来实现这个逻辑 - 继承自`React.PureComponent`，大多数情况下，不必手写`shouldComponentUpdate`
~~~js
import React from 'react';

class PureComponent extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(prevState => ({count: prevState.count + 1}))}
      >
        {this.state.count}
      </button>
    );
  }
}

export default PureComponent;
~~~

>React.PureComponent，它只做一个浅比较。但是由于浅比较会忽略属性或状态突变的情况，此时你不能使用它

~~~js
import React from 'react';

class ListOfWords extends React.PureComponent {
  render() {
    return (<div>{this.props.words.join(',')}</div>);
  }
}

class WordAdder extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: ['abc']
    };
  }

  handleClick = () => {
    // This section is bad style and causes a bug
    const words = this.state.words;
    words.push('def');
    this.setState({
      words
    });
    // This section is good style
    this.setState(prevState => ({
      words: prevStae.words.concat(['def'])
    }));
    // This section is good style
    this.setState(prevState => ({
      words: [...prevState.words, 'def']
    }));
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>click</button>
        <ListOfWords words={this.state.words}></ListOfWords>
      </div>
    );
  }
}

export default WordAdder;
~~~

~~~js
import React from 'react';

class SubComponent extends React.PureComponent {
  componentDidMount() {
    console.log('words', this.props.words);
  }
  
  render() {
    return (
      <div>
        <span>这是个span</span>
        {this.props.words.center}
      </div>
    );
  }
}

class ImmutableObject extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: {
        left: 'red',
        right: 'blue'
      }
    };
  }

  handleClick = () => {
    const words = this.state.words;
    words.center = 'yellow';
    // This section is bad style and causes a bug
    this.setState({
      words
    })
    // This section is good style
    this.setState(prevState => ({
      words: Object.assign({}, prevState.words, {center: 'yellow'})
    }));
    // This section is good style
    this.setState(prevState => ({
      words: {...prevState.words, center: 'yellow'}
    }))
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>object click</button>
        <SubComponent words={this.state.words}></SubComponent>
      </div>
    );
  }
}

export default ImmutableObject;
~~~