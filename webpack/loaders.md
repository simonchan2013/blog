### 对应链接
  - [Loaders](http://webpack.github.io/docs/loaders.html)
  - [How to write a loader](http://webpack.github.io/docs/how-to-write-a-loader.html)
  - [Using loaders](http://webpack.github.io/docs/using-loaders.html)



### 基本概念
- 用途：在加载文件前进行预处理

- `loader`特性
  - 可以链接起来，将资源在管道中传递，对除最后一个之外（放在最前面）的所有`loader`，可返回任意格式的结果传递给下一个`loader`，最后一个将返回`JavaScript`
  - 可以是异步的，也可以是同步的
  - 在`Node.js`中运行
  - 接受参数
  - 可以通过后缀名（包括正则表达式格式）绑定到指定文件
  - 插件（`plugins`）可以扩展`loader`的特性

- 几种设置`loader`的方法
  - 在加载文件时指定

  ```js
  var moduleWithLoader1 = require('my-loader!./my-module-1');
  var moduleWithLoader2 = require('./loaders/my-loader!./my-module-1');

  // 多个`loader`可用`!`隔开，形成`loader chain`，加载时的处理顺序为从右向左
  require('style-loader!css-loader!less-loader!./my-styles.less');

  // 带参数（query parameters）
  require('loader?param1=123!./file');
  ```

  - 在配置文件（如`webpack.config.js`）中配置
  ```js
  {
    module: {
      loaders: [
        { test:/\.coffee$/, loader:'coffee-loader' }
      ],
      preLoaders: [
        { test:/\.coffee$/, loader:'coffee-hint-loader' }
      ]
    }
  }
  ```

- `loader`处理顺序
  1. 配置文件中指定的`preLoaders`
  2. 配置文件中指定的`loaders`
  3. 加载文件时指定的loader（即：`require('raw!./file.js')`）
  4. 配置文件中指定的`postLoaders`

- 覆盖配置文件中的指定的`loaders`

  ```js
  // 禁用`preLoaders`，在开头添加`!`
  require('!raw!./script.coffee');

  // 禁用所有`loaders`，在开头添加`!!`
  require('!!raw!./script.coffee');

  // 禁用`preLoaders`和`loaders`，但`postLoaders`不受影响，在开头添加`-!`
  require(`-!raw!./script.coffee`);
  ```



### 开发一个`loader`
