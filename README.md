## webpack入门

### 初始化项目
1. 安装webpack环境
```bash
npm install webpack webpapck-cli --save-dev
```
2. 新建webpack.config.js
> webpack的默认入口是src/index.js，默认出口是dist/main.js，此处将修改入口为src/app.js，出口为dist/app.bundle.js
3. 安装CleanWebpackPlugin和HtmlWebpackPlugin插件，用于自动清理生成的dist文件夹和自动绑定生成的js
```bash
npm install clean-webpack-plugin html-webpack-plugin --save-dev
```

webpack.config.js内容如下
```js
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');
var { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/app.js',
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './index.html'
    })
  ]
}
```
