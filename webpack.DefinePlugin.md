可以这么理解，通过配置DefinePlugin，里面设置的常量就是全局，可以在业务js代码访问到。
比如:
	webapck.config.js
``` javascript
	plugins:[
		new webpack.DefinePlugin({
			__DEV__:true
		})
	]
```
xx.js
``` javascript
	if(__DEV__){
		//development do something...
	}else{
		//production do something...
	}
```
react打包的README.md 内，有一段说明：
###### by default, React will be in development mode. The development version includes extra warnings about common mistakes, whereas the production version includes extra performance optimizations and strips all error messages.

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;意思是说：默认react是开发环境，包括了错误警告信息。而生产版本则拥有额外的性能优化和删除了错误信息提示。

------------

###### To use React in production mode, set the environment variable `NODE_ENV` to `production`. A minifier that performs dead-code elimination such as [UglifyJS](https://github.com/mishoo/UglifyJS2) is recommended to completely remove the extra code present in development mode.

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大意：在生产环境使用react，设置环境变量  NODE_ENV 成' production '。他是一个做了代码混淆，完整删除开发模式中存在的额外代码。
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;那么我们可以利用webpack的DefinePlugin来区分这两种模式。


```
	new webpack.DefinePlugin({
		  'process.dev':{
		  		'NODE_DEV':JSON.stringify(process.env.NODE_DEV)
		  }
	})
```
key里面的process.dev是给前端的
value里面的precess.env 是node端的
这两个要区分好。之所以key里为什么这么写，这个是react拟的一个名字，在源码里通过这个key里面的值做判断是否是development environment 还是 production environment。


