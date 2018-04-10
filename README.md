# webpack4-es6-demo
this is a webpack4 es6 demo

第一步安装依赖
1.创建文件夹 mkdir test1 & cd test1
2.yarn init -y,得到package.js
3.全局安装：yarn global add webpack webpack-cli 
4.本地安装： yarn add -D webpack webpack-cli
5.在test1目录下新建test1/src/index.js文件，作为默认入口文件
6.修改package.js配置如下
package.js
{
  "name": "test1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --mode development --output-public-path dist --open",
    "dev": "webpack --mode development --watch ",
    "build": "webpack --mode production"
  },
  "dependencies": {
    "jquery": "^3.2.1"
  },
  "author": "homer",
  "license": "MIT",
  "devDependencies": {
    "webpack": "^4.5.0",
    "webpack-cli": "^2.0.14"
  }
}
7.命令的使用
在使用命令 yarn run start开启server，
使用yarn run dev开启开发模式下的打包并监听，
使用yarn run build进行生产模式下的打包，
第二步，创建配置文件webpack.config.js
1.千万别把名字写成 webpack-config.js (犯了错，折腾半天）
2.先添加依赖
yarn add -D babel-core babel-loader babel-preset-env clean-webpack-plugin css-loader html-webpack-plugin style-loader
强调（es6-babel处理：在package.js里添加"babel":{ "presets": ["env"] }, 或者在.babelrc里添加{ "presets": ["env"]}都可以。二选一。）
package.js
{
  "name": "test1",
  "version": "1.0.0",
  "description": "a project base on angular 1.x and webpack",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --mode development --output-public-path dist --open",
    "dev": "webpack --mode development --watch ",
    "build": "webpack --mode production"
  },
  "dependencies": {
    "jquery": "^3.2.1"
  },
  "babel":{
    "presets": ["env"]
  },
  "author": "homer",
  "license": "MIT",
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.4",
    "babel-preset-env": "^1.6.1",
    "clean-webpack-plugin": "^0.1.19",
    "css-loader": "^0.28.11",
    "html-webpack-plugin": "^3.2.0",
    "style-loader": "^0.20.3",
    "webpack": "^4.5.0",
    "webpack-cli": "^2.0.14"
  }
}
3.创建webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  entry: { 
    app: './src/index.js',   
    hello: './src/hello.js'
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [{
      test: /\.js$/,
      include: [path.resolve(__dirname, 'src')],
      loader: 'babel-loader'
    },
    {
      test: /\.css$/,
      use: [
      "style-loader",
      "css-loader"
      ]
    }
    ]
  },
  plugins: [
  new CleanWebpackPlugin(['dist']),
  new HtmlWebpackPlugin({
    title: 'Output Management'
  })
  ]
}
多入口配置app和hello会生成多个出口文件app.bundle.js、hello.bundle.js
new CleanWebpackPlugin会清理旧的dist下文件，
new HtmlWebpackPlugin会自动生成dist/index.html文件

