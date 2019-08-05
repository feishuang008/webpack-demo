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

### CSS模块化

#### 安装stylel-loader和css-loader
```bash
npm install style-loader css-loader --save-dev
```

#### 配置loaser
```js
{
  test: /\.css$/,
  use: [
    'style-loader',
    {
      loader: 'css-loader',
      options: {
        modules: {
          localIdentName: '[local]-[hash:base64:5]'
        }
      }
    }
  ]
}
```

#### localIdentName有如下取值（来自loader-utils）
* `[ext]` the extension of the resource
* `[name]` the basename of the resource
* `[path]` the path of the resource relative to the `context` query parameter or option.
* `[folder]` the folder the resource is in.
* `[emoji]` a random emoji representation of `options.content`
* `[emoji:<length>]` same as above, but with a customizable number of emojis
* `[contenthash]` the hash of `options.content` (Buffer) (by default it's the hex digest of the md5 hash)
* `[<hashType>:contenthash:<digestType>:<length>]` optionally one can configure
  * other `hashType`s, i. e. `sha1`, `md5`, `sha256`, `sha512`
  * other `digestType`s, i. e. `hex`, `base26`, `base32`, `base36`, `base49`, `base52`, `base58`, `base62`, `base64`
  * and `length` the length in chars
* `[hash]` the hash of `options.content` (Buffer) (by default it's the hex digest of the md5 hash)
* `[<hashType>:hash:<digestType>:<length>]` optionally one can configure
  * other `hashType`s, i. e. `sha1`, `md5`, `sha256`, `sha512`
  * other `digestType`s, i. e. `hex`, `base26`, `base32`, `base36`, `base49`, `base52`, `base58`, `base62`, `base64`
  * and `length` the length in chars
* `[N]` the N-th match obtained from matching the current file name against `options.regExp`
