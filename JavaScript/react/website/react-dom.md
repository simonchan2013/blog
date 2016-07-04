# React DOM

### 对应链接
[React (Virtual) DOM Terminology](https://facebook.github.io/react/docs/glossary.html)



### React Elements
- DOM Element的一个虚拟表示
- 4个属性：`type`、`props`、`key`、`ref`
- Example

  ```js
  var child = React.createElement('li', null, 'Text Content');
  var root = React.createElement('ul', { className: 'my-list' }, child);
  ReactDOM.render(root, document.getElementById('example'));

  // or in jsx
  var root = <ul className="my-list"><li>Text Content</li></ul>;
  ReactDOM.render(root, document.getElementById('example'));
  ```

- React Element Factories：创建出一个工厂函数，该函数根据type创建`ReactElement`，例子如下

  ```js
  var div = React.CreateFactory('div');
  var root = div({ className:'my-div' });
  ReactDOM.render(root, document.getElementById('example'));
  ```



### React Nodes
一个ReactNode可以是：`ReactElement`、`string`(`ReactText`)、`number`(`ReactText`)、`ReactNode`数组(`ReactFragment`)



### React Components
- 一个`ReactComponent Class`就是一个JavaScript Class（或叫构造函数），可通过`React.createClass`创建
- 当`ReactComponent Class`被调用时，会返回一个至少包含一个`render`函数的对象
- `ReactComponent Class`可以作为参数被传给`createElement`，生成的`ReactElement`可以通过`ReactDOM.render`创建一个`ReactComponent`
- Example

  ```js
  // ReactComponent Class
  var MyComponent = React.createClass({
    render() {
      return (
        <div>
          <h1>Component Test</h1>
          <p>This is a component test</p>
        </div>
      );
    }
  });

  var element = React.createElement(MyComponent);
  // or in jsx
  // var element = <MyComponent />;

  // render a ReactComponent
  var component = ReactDOM.render(element, document.getElementById('example'));
  ```
