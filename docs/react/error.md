# error collections

## setState方法
~~~js
this.setState = {
  name: 'dzh'
};
~~~

>Warning: React DevTools encountered an error: TypeError: inst.setState.bind is not a function

~~~js
this.setState({
  name: 'dzh'
})
~~~

## Object React Child

>Warning: Objects are not valid as a React child (found: object with keys {left, right}). If you meant to render a collection of children, use an array instead.

~~~js
render() {
  return (
    <div>
      {this.props.words}
      <span>这是个span</span>
    </div>
  );
}
~~~
