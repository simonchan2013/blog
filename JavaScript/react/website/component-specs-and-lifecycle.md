### 对应链接
- 官网
  - [Component Specs and Lifecycle](https://facebook.github.io/react/docs/component-specs.html)
  - [Reusable Components](https://facebook.github.io/react/docs/reusable-components.html)

- 其他
  - [Why React PropTypes are important](http://wecodetheweb.com/2015/06/02/why-react-proptypes-are-important/)
  - [React component’s lifecycle.](https://medium.com/react-ecosystem/react-components-lifecycle-ce09239010df#.bj12kp9zp)


### Component Specifications
调用`React.createClass()`时作为参数传入，该对象包含：
  - `render()`函数（必需）
  - 其他生命周期函数（可选）
  - 其他对象及函数（可选，包括一些初始化函数、静态函数块等）



### render
- 检测`this.props`和`this.state`
- 返回一个`React Component`，也可返回`null`或`false`表示不渲染任何内容
- 当返回`null`或`false`时，React将渲染一个`<noscript>`标签，且`ReactDOM.findDOMNode(this)`将返回`null`
- 纯函数，不应该修改组件的状态，每次调用都返回同样的结果
- 不读取以及操作DOM，不与浏览器相互作用，这些都应该放在生命周期函数中进行



### Other Specifications Properties
- `getInitialState()`：组件装载前调用一次，返回的`object`将作为`this.state`的初始值。使用ES6类时，这个方法的功能被`constructor`代替

- `getDefaultProps()`：创建class时调用一次（在使用该class创建实例前调用）并缓存结果（`object`）
  - 当`parent component`没有传入指定的属性时，返回的结果用于设置`this.props`中对应的值
  - 该函数不能依赖`this.props`
  - 返回的结果将被所有实例共享
  - 使用ES6类时，这个方法的功能被`constructor`代替
  - [Example](https://facebook.github.io/react/docs/reusable-components.html#default-prop-values)

- `propTypes`：验证传入的属性
  - 提供一系列验证器，详见[Prop Validation](https://facebook.github.io/react/docs/reusable-components.html#prop-validation)
  - 如果验证不通过，会在console中输出一个警告，仅在development模式下

- `mixins`：数组，在多个组件内共享行为，通用方法

- `statics`：`object`，在该对象内可自定义一系列通用的静态函数
  - 不依赖组件，可以在任何实例被创建之前运行
  - 不访问以及操作组件的`props`和`state`

- `displayName`：用于debug信息



### Lifecycle Methods
- Initialization
  - `getInitialState()`
  - `getDefaultProps()`
- Mounting：组件将被插入DOM中
  - `componentWillMount()`
    - 在最初渲染组件并加载之前调用（即刚进入页面时），即在render前执行
    - 可在服务端渲染前或客户端渲染前调用，只调用一次
    - 在这个方法中调用`this.setState()`不会触发`componentWillUpdate(nextProps, nextState)`，即不会重渲染
  - `componentDidMount()`
    - 在最初渲染组件并加载之后调用（即刚进入页面时），即在render后执行
    - 仅在客户端调用，只调用一次
    - 在这个时候可以访问到`children`的`refs`
    - 子组件的`componentDidMount()`方法比父组件的`componentDidMount()`先调用
    - 一般在这个方法中请求组件需要的数据
    - 在这个方法中调用`this.setState()`将触发`componentWillUpdate(nextProps, nextState)`

- Updating：重渲染组件并决定DOM是否应该更新
  - `componentWillReceiveProps(nextProps)`
    - 当组件接收新`props`时调用，组件首次渲染时不会被调用
    - 在该方法中调用`this.setState()`不会触发额外的渲染
  - `shouldComponentUpdate(nextProps, nextState)`
    - 默认返回`true`，即默认会重新渲染
    - 可在这个方法中加入逻辑，避免不必要的重渲染
  - `componentWillUpdate(nextProps, nextState)`
    - 在重渲染之前被调用
    - 组件首次渲染不会调用这个方法
    - 不能在该方法内使用`this.setState()`，如果需要更新`state`，在`componentWillReceiveProps(nextProps)`内处理
  - `componentDidUpdate(prevProps, prevState)`
    - 在重渲染之后调用
    - 组件首次渲染不会调用这个方法
    - 在此时，可以开始对DOM进行交互处理
    - [Example](https://gist.github.com/osmelmora/eb46bc2665c3255b109b31e56427b37d#file-select2_wrapper-js)：重渲染后，对Select下拉菜单进行更新
- Unmounting：组件将被从DOM中移除
  - 在组件即将移出DOM时执行
  - 一般在这里做一些清理工作，如移除定时器、destroy一些元素等。
