# koa2-webpack-middleware

koa2 middleware with HMR(hot module replacement) supports

依赖

[webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)

[webpack-hot-middleware](https://github.com/glenjamin/webpack-hot-middleware)

修改自

[koa-webpack-middleware](https://github.com/leecade/koa-webpack-middleware)

## Install

```sh
$ npm install koa2-webpack-middleware -D
```

## Usage

```js
import webpack from 'webpack'
import { devMiddleware, hotMiddleware } from 'koa-webpack-middleware'
import devConfig from './webpack.config.dev'
const compile = webpack(devConfig)
app.use(devMiddleware(compile, {
    // display no info to console (only warnings and errors)
    noInfo: false,

    // display nothing to the console
    quiet: false,

    // switch into lazy mode
    // that means no watching, but recompilation on every request
    lazy: true,

    // watch options (only lazy: false)
    watchOptions: {
        aggregateTimeout: 300,
        poll: true
    },

    // public path to bind the middleware to
    // use the same as in webpack
    publicPath: "/assets/",

    // custom headers
    headers: { "X-Custom-Header": "yes" },

    // options for formating the statistics
    stats: {
        colors: true
    }
}))

app.use(hotMiddleware(compile, {
  // log: console.log,
  // path: '/__webpack_hmr',
  // heartbeat: 10 * 1000
}))
```

## HMR configure

1. webpack `plugins` configure

    ```js
    plugins: [
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoErrorsPlugin()
    ]
    ```
2. webpack `entry` configure

    ```js
    entry: {
      'index': [
        'webpack-hot-middleware/client?path=/__webpack_hmr&timeout=20000',
        'index.js']
    },
    ```

3. put the code in your entry file to enable HMR

    > Pure JS need

    ```js
    if (module.hot) {
      module.hot.accept()
    }
    ```

更多配置说明参考 

[webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)

[webpack-hot-middleware](https://github.com/glenjamin/webpack-hot-middleware)
