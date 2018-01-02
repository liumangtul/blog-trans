webpack.config.dev.js

```
const path = require('path');
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const OpenBrowserPlugin = require('open-browser-webpack-plugin');

module.exports = {
  entry:path.resolve(__dirname,'src/index.jsx'),
  output:{
    filename:'bundle.js'
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  module:{
    loaders:[
      {
        test:/\.(js|jsx)?$/,
        exclude:/node_modules/,
        loader:'babel-loader',
        query: {
            presets: ['es2015','react','stage-0']
        }
      },{
        test:/\.less$/,
        exclude:/node_modules/,
        loader:'style-loader!css-loader!less-loader!postcss-loader'
      },{
        test:/\.css$/,
        exclude:/node_modules/,
        loader:'style-loader!css-loader!postcss-loader'
      },{
        test:/\.(png|gif|jpg|jpeg|bmp)$/i,
        loader:'url-loader?limit=5000' // 限制大小小于5k
      },{
        test:/\.(png|woff|woff2|svg|ttf|eot)($|\?)/i,
        loader:'url-loader?limit=5000' // 限制大小小于5k
      }
    ]
  },
  plugins:[
    // 打开浏览器
    new OpenBrowserPlugin({
      url: 'http://localhost:8080'
    }),
    new HtmlWebpackPlugin({
      template:__dirname + '/src/index.html'
    }),
    new webpack.HotModuleReplacementPlugin(),
    // 可在业务js代码中使用__DEV__判断是否是development environment.
    new webpack.DefinePlugin({
      __DEV__: JSON.stringify(JSON.parse((process.env.NODE_ENV === 'dev') || 'false'))
    })
  ],
  devServer:{
    // contentBase: './',
    inline:true,
    hot:true
  }
};

```


------------

webpack.config.production.js
```
const path = require('path');
const package = require('./package.json');
const webpack = require('webpack');
const ExtractTextWebpackPlugin = require('extract-text-webpack-plugin');
const htmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
  entry:{
    app:path.resolve(__dirname,'src/index.jsx'),
    vendor:Object.keys(package.dependencies)
  },
  output:{
    path:__dirname+'/build',
    filename:'js/[name][hash:8].js',
    publicPath:'./'
  },
  devtool:'inline-source-map',
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  module:{
    loaders:[
      //处理js
      {
        test:/\.(js|jsx)?$/,
        exclude:/node_modules/,
        loader:'babel-loader',
        //依赖
        query:{
          presets:['es2015','react','stage-0']
        }
      },
      //处理less
      {
        test:/\.less$/,
        exclude:/node_modules/,
        loader:ExtractTextWebpackPlugin.extract({
          fallback:'style-loader',
          use:'css-loader!postcss-loader!less-loader'
        })
      },
      //处理css
      {
        test:/\.css$/,
        exclude:/node_modules/,
        loader:ExtractTextWebpackPlugin.extract({
          fallback:'style-loader',
          use:'css-loader!postcss-loader'//?name=../images/[hash:8].[name].[ext]'
        })
      },
      //处理图片
      {
        test:/\.(png|gif|jpg|jpeg|bmp)$/,
        exclude:/node_modules/,
        loader:'url-loader',//?limit=5000&name=./images/[name].[hash:8].[ext]
        query:{
          limit:5000,
          name:'images/[name].[hash:8].[ext]',
          //outputPath:'../'
          //useRelativePath:true
        }
      },
      {
        //处理字体
        test:/\.(png|woff|woff2|svg|ttf|eot)($|\?)/i,
        loader:'url-loader',
        query:{
          limit:5000,
          name:'fonts/[name][hash:8].[ext]'
        }
      }
    ]
  },
  plugins:[
    //BannerPlugin
    new webpack.BannerPlugin("Copyright by wangyan"),
    //clean file
    new CleanWebpackPlugin(['build/']),
    //html 模板
    new htmlWebpackPlugin({
      template:__dirname + '/src/index.html'
    }),
    //为组件分配ID，分析和优先考虑使用最多的模块，并为他们分配最小id
    new webpack.optimize.OccurrenceOrderPlugin(),
    //代码混淆压缩
    new webpack.optimize.UglifyJsPlugin({
      compress:{
        // 去除警告代码
        warnings:false
      },
      // sourceMap: true//,
      // mangle: true
    }),
    //分离css
    new ExtractTextWebpackPlugin('./[name].[hash:8].css'),
    //提取公共代码
    new webpack.optimize.CommonsChunkPlugin({
      name:'vendor',
      filename:'/js/[name].[hash:8].js'
    }),
    new webpack.DefinePlugin({
      //react
      'process.dev':{
        'NODE_DEV':JSON.stringify(process.env.NODE_DEV)
      },
      // 可在业务js代码中使用__DEV__判断是否是dev模式
      __DEV__:JSON.stringify(JSON.parse((process.env.NODE_DEV === 'dev') || 'false'))
    })
  ]
}

```


------------

package.json
```
{
  "name": "react-scroll",
  "version": "0.0.1",
  "description": "react scroll",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "NODE_ENV=dev webpack-dev-server --colors --progress --config ./webpack.config.dev.js -d",
    "build": "rm -rf ./build && NODE_DEV=production webpack --config ./webpack.config.production.js --progress"
  },
  "author": "promise",
  "license": "ISC",
  "keywords": [
    "iscroll",
    "react"
  ],
  "dependencies": {
    "react": "^16.1.1",
    "react-dom": "^16.1.1"
  },
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1",
    "clean-webpack-plugin": "^0.1.17",
    "css-loader": "^0.28.7",
    "extract-text-webpack-plugin": "^3.0.2",
    "file-loader": "^1.1.5",
    "html-webpack-plugin": "^2.30.1",
    "less": "^2.7.3",
    "less-loader": "^4.0.5",
    "open-browser-webpack-plugin": "0.0.5",
    "postcss-loader": "^2.0.9",
    "react-redux": "^5.0.6",
    "redux": "^3.7.2",
    "style-loader": "^0.19.0",
    "url-loader": "^0.6.2",
    "webpack": "^3.8.1",
    "webpack-dev-server": "^2.9.5"
  }
}

```

------------

打包时，如果用 webpack -p 压缩混淆会报错，看结果报错应该是有些变量压缩后出错。顾只使用：
```
//代码混淆压缩
    new webpack.optimize.UglifyJsPlugin({
      compress:{
        // 去除警告代码
        warnings:false
      },
      // sourceMap: true//,
      // mangle: true
    })
```

##### 因为经过测试发现，UglifyJsPlugin和-p 不能同时使用，且UglifyJsPlugin压缩率高出一倍多。

------------
output在production 环境与development 环境下有区别，output在production环境下把框架、插件代码与自己的业务代码分开，plugins里，增加了代码压缩混淆，去除warnings部分，处理css代码剥离合并，提取公共代码，定义process.env.NODE_DEV 给react用。
loaders里对于less，css的处理也有不同，主要是做了代码分离。
其他两个环境的区别就不一一说了。当然随着项目的日渐庞大，配置会有不同。