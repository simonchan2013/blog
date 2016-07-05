### 对应链接
- 官网
  - [Component Specs and Lifecycle](https://facebook.github.io/react/docs/component-specs.html)
  - [Reusable Components](https://facebook.github.io/react/docs/reusable-components.html)

- 其他
  - [Why React PropTypes are important](http://wecodetheweb.com/2015/06/02/why-react-proptypes-are-important/)


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
- `getInitialState()`：组件装载前调用一次，返回的`object`将作为`this.state`的初始值

- `getDefaultProps()`：创建class时调用一次（在使用该class创建实例前调用）并缓存结果（`object`）
  - 当`parent component`没有传入指定的属性时，返回的结果用于设置`this.props`中对应的值
  - 该函数不能依赖`this.props`
  - 返回的结果将被所有实例共享
  - [Example](https://facebook.github.io/react/docs/reusable-components.html#default-prop-values)

- `propTypes`：验证传入的属性
  - 提供一系列验证器，详见[Prop Validation](https://facebook.github.io/react/docs/reusable-components.html#prop-validation)
  - 如果验证不通过，会在console中输出一个警告，仅在development模式下

- `mixins`：数组，在多个组件内共享行为

- `statics`：`object`，在该对象内可自定义一系列通用的静态函数
  - 不依赖组件，可以在任何实例被创建之前运行
  - 不访问以及操作组件的`props`和`state`

- `displayName`：用于debug信息



### Lifecycle Methods
- Mounting
  - `componentWillMount()`
  - `componentDidMount()`
- Updating
- Unmounting
