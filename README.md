# Flow Babel Webpack Plugin

A concise tool that glues together [`Flow`](https://flowtype.org/) and [`Webpack`](https://webpack.github.io/), with the help of [`Babel`](https://babeljs.io/).

It provides you with flow typecheck status in webpack build reports.

---

### Usage

Since JS and `Flow` syntax vary slightly, you will need to get rid of the type annotations.

This is where [`transform-flow-comments`](https://babeljs.io/docs/plugins/transform-flow-comments/) comes in. It converts flow type annotations into comments that `Flow` [understands](https://flowtype.org/blog/2015/02/20/Flow-Comments.html).

You need to follow a few simple steps.

#### 1. Install dependencies

```sh
# Install Babel and Webpack and save as devDependencies
npm i -D babel-core babel-loader webpack

# Install FBWP
npm i -D flow-babel-webpack-plugin
```

#### 2.  Setup babel and flow
```sh
# setup .flowconfig
./node_modules/.bin/flow init  or flow init

# .babelrc file
{
  "plugins" : [
    "transform-flow-comments"
  ]
}
```

#### 3. Setup webpack config

```js
// webpack.config.js file

var FlowBabelWebpackPlugin = require('flow-babel-webpack-plugin');

module.exports = {
  entry: './index',

  output: {
    filename: 'build.js',
  },

  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel',
      },
    ],
  },

  plugins: [
    new FlowBabelWebpackPlugin(),
  ],
}
```

#### And that's it!

From now on, when you run webpack, you will recieve flow status reports alongside your webpack build log.

Something like this.
![](/demo.png)

#### Options

It should work pretty well with the defaults, but there are some options available:

##### warn

If you'd prefer to treat Flow issues as webpack warnings instead of errors, you can enable this option.

```js
plugins: [
  new FlowBabelWebpackPlugin({
    warn: true,
  }),
],
```

##### formatter

You can provide your own error message formatting function in order to customize the output.

For example:
```js
plugins: [
  new FlowBabelWebpackPlugin({
    formatter: function (errorCode, errorDetails) {
      return 'A Flow error was detected: ' + errorCode + '\n\n' + errorDetails;
    },
  }),
],
```

---

### What's next?

Nothing much, really. All I wanted was to display flow reports alongside webpack's - nothing fance.

I might add something more to it, if I find it really useful. Some options are:

* IO redirection for further logging or processing
* External file checks, i.e., files that lie outside of project's root folder

If you have something in mind, or something you want, feel free to [ask](https://github.com/zhirzh/flow-babel-webpack-plugin/issues).
