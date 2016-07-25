### 对应链接
  - [configuration](http://webpack.github.io/docs/configuration.html)



### 目前用到过的配置项
- `context`：指定`entry`的基本目录（绝对路径），默认为`process.cwd()`。指定后， `entry`、 `output.pathinfo`的相对路径都是以这个目录为根目录。

- `entry`：打包入口，一个相对于`context`的路径（也可能是几个，数组格式，或是对象格式）
  - 如果传入一个字符串，该字符串所指定的模块将在启动时加载
  - 如果传入一个数组，其中包含的所有模块将在启动时加载，且最后一个将被生成的`chunk`导出
  - 如果传入一个对象，可能会被打包成多个`chunk`
  - Example

    ```js
    {
      entry: {
        page1: './page1',
        page2: ['./entry1', './entry']
      },
      output: {
        // 当有多个入口时，用`[name]`或`[id]`作为`filename`，否则会打包到同一个文件
        filename: '[name].bundle.js',
        chunkFilename: '[id].bundlue.js'
      }
    }
    ```

- `output`：指定打包编译后输出文件的一系列配置
  - `filename`：输出文件名
    - 单一入口可以直接用字符串指定文件名
    - 多个入口则需要使用`[name]`、`[hash]`、`[id]`或`[chunkhash]`作为文件名（或文件名的一部分）以确保打包成多个文件
      - `[name]`：`chunk`指定的入口名
      - `[hash]`：编译的hash值
      - `[chunkhash]`：`chunk`的hash值

  - `path`：输出文件在项目中的位置，默认是`process.cwd()`。也是使用`[hash]`

  - `publicPath`：指定浏览器引用打包结果资源的地址
    - 如果在编译的阶段不能确定最终的`pablicPath`，可以在`entry point`的文件中指定`__webpack_public_path__`
    - Example
      - config.js

        ```js
        output: {
          path: '/home/proj/public/assets',
          publicPath: '/assets/'
        }
        ```

      - index.html

        ```html
        <head>
          <link href="/assets/spinner.gif"/>
        </head>
        ```

      - cdn with hash example config.js

        ```js
        output: {
          path: "/home/proj/cdn/assets/[hash]",
          publicPath: "http://cdn.example.com/assets/[hash]/"
        }
        ```
    - `library`：如果设置，打包的结果将会被作为一个库导出，`library`则是该库的名字

    - `libraryTarget`：最后导出库的格式
      - `var`：作为变量导出，`var Library = xxx`（默认）
      - `commonjs2`：作为commonjs模块导出，`module.exports = xxx`

- `module`：常规模块的相关配置
  - `loaders`：一系列自动应用的`loader`，数组形式，每个元素都是如下格式的对象：
    - 可包含的元素：
      - `test`：使用该`loader`所必须满足的条件，正则表达式，一般用于指定文件后缀名
      - `exclude`：指定排除的的条件，正则表达式，一般用于指定排除某些目录，如：`node_modules`
      - `include`：必须满足的条件，一般用于指定作用的目录
      - `loader`：一系列要处理在满足上面三个条件的文件的`loader`，多个`loader`间用`!`隔开
      - `loaders`：一个关于`loader`的数组
    - 以上提到的3个条件可以是正则表达式、字符串、一个返回布尔值的函数、由前面三项组成的数组
    - 关于`loader`的相关内容可查看[Loaders](http://webpack.github.io/docs/loaders.html)
    - Example

      ```js
      module.loaders: [
        {
          test: /\.jsx$/,
          include: [
            path.resolve(__dirname, 'app/src'),
            path.resolve(__dirname, 'app/test')
          ],
          exclude: /node_modules/,
          loader: 'babel-loader'
        }
      ]
      ```

  - `preLoader`：指定[`preLoader`](http://webpack.github.io/docs/loaders.html#loader-order)的配置

  - `postLoader`：指定[`postLoader`](http://webpack.github.io/docs/loaders.html#loader-order)的配置

  - `plugins`：添加编译时使用的插件，数组

  - `target`：指定编译成哪个运行环境下的文件
    - `web`：浏览器环境（默认）
    - `webwoker`
    - `node`：类`node.js`环境，用`require`加载`chunk`
    - `async-node`：类`node.js`环境，用`fs`和`vm`异步加载`chunk`
    - `node-webkit`
    - `electron`
