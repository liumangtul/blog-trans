## 一、配置postcss-loader时，总是出现未配置postcss的问题： ##
经研究有两种方法：
### 1:
```javascript
module:{
      plugins:[
          ...
          new webpack.LoaderOptionsPlugin({
              optoins:{
                  postcss: function() {
                      return [
                          //调用autoprefixer插件，例如 display: flex
                          require('autoprefixer')
                      ];
                  }
              }
          })
      ]
}
```
但是这种方法实测，对webpack3无效。
### 2.
新建一个postcss.config.js 和 webpack.config.js同级
```javascript
module.exports = {
  plugins: {
    'autoprefixer': {browsers: 'last 5 version'}
  }
}
```
实测有效。


----------
## 二、
//为组件分配id，分析和优先考虑使用最多的模块，并为他们分配最小ID
```javascript
    // new webpack.optimize.OccurenceOrderPlugin(),
    new webpack.optimize.OccurrenceOrderPlugin(),
```
webpack3 的不再是OccurenceOrderPlugin，更改为OccurrenceOrderPlugin了。
## 三、关于chunkhash
chunkhash更改为hash