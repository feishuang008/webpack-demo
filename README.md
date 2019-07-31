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

### webpack解析CSS和SCSS
1. 安装相关依赖
```
npm install style-loader css-loader --save-dev
npm install sass-loader node-sass --save-dev
npm install postcss-loader autoprefixer --save-dev
```
2. 配置css-loader
```js
{
  test: /\.css$/,
  use: [
    'style-loader', // 将CSS以style的形式载入到HTML中
    'css-loader'    // 解析CSS
  ]
}
```
3. 配置scss-loader（loader的顺序是从右往左执行）
```js
{
  test: /\.scss$/,
  use: [
    'style-loader',
    'css-loader',
    {
      loader: 'postcss-loader',
      options: {
        plugins: [require('autoprefixer')({
          browsers: ['> 1%', 'last 2 versions'] // 自动添加后缀
        })]
      }
    },
    'sass-loader'
  ]
}
```
参考链接：[在webpack中使用autoprefixer](https://www.jianshu.com/p/46a19ee297fc);


此时会给出一个警告
```dos
  Replace Autoprefixer browsers option to Browserslist config.
  Use browserslist key in package.json or .browserslistrc file.

  Using browsers option cause some error. Browserslist config
  can be used for Babel, Autoprefixer, postcss-normalize and other tools.

  If you really need to use option, rename it to overrideBrowserslist.

  Learn more at:
  https://github.com/browserslist/browserslist#readme
  https://twitter.com/browserslist
```
此警告的意思就是推荐在package.json中使用brwoserslist字段，或者使用.browerslistrc文件来进行配置，可以通用到babel、autoprefixer和postcss-normalize等工具。

> 我们将postcss-loader的配置提取到postcss.config.js中，如下

```js
module.exports = {
  plugins: [require('autoprefixer')]
}
```
> 新建.browserslistrc文件，内容如下

```bash
> 1%
last 2 versions
android >= 4
ios >= 6
```
此时，scss的配置可以简化为
```js
{
  test: /\.scss$/,
  use: [
    'style-loader',
    'css-loader',
    'postcss-loader',
    'sass-loader'
  ]
}
```
最后，如果想跟踪css的源文件，可以对css-loader做出调整
```js
{
  loader: 'css-loader',
  options: {
    sourceMap: true
  }
}
```
