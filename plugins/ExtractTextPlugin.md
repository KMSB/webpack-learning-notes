#ExtractTextPlugin（[github](https://github.com/webpack/extract-text-webpack-plugin)）
##可以将你的样式提取到单独的css文件里
###优点：
* 更少的 style 标签（ie 下会有限制）
* 可以使用 CSS source-map
* CSS 文件可以同时被请求
* CSS 被分开缓存
* 执行的更快 
###缺点：
* 额外的HTTP请求
* 更长的编译时间
* 配置更加复杂
* No runtime public path modification
* No Hot Module Replacement
###API：
```
new ExtractTextPlugin([id: string], filename: string, [options])
```
  * id：为此插件实例指定一个id，默认是自动生成的
  * filename：生成文件的文件名，可包含以下几部分
    * [name]：此chunk的名字
    * [id]：此chunk的id
    * [contenthash]：根据文件内容生成的hash值
  * options：
    * allChunks：设置为 true 时：将所有 js 中 require 到的样式表都从各自的 js 中提取出一个个的 css 文件，默认只从入口文件中提取，而非入口文件则会将 css 内嵌到 js 中
    * disable：禁用此插件
  ExtractTextPlugin只会为每个入口文件生成一个输出文件，当使用多入口时，请务必为文件名指定[name]、[id]或者[contenthash]
```
ExtractTextPlugin.extract([notExtractLoader], loader, [options])
```
  * [notExtractLoader ]当样式不被提取要使用的 loader （当 allChunks 为 false 时可能发生这种情况）
  * loader：需要对 css 预处理的 loader ，如 [css!less] 
  * [options]：选项
    * publicPath : 覆盖 publicPath 配置
###实例配置：
```
let ExtractTextPlugin = require('extract-text-webpack-plugin'); 
// multiple extract instances
let extractCSS = new ExtractTextPlugin('stylesheets/[name].css');
let extractLESS = new ExtractTextPlugin('stylesheets/[name].less');
 
module.exports = {
...
  module: {
    loaders: [
      {test: /\.scss$/i, loader: extractCSS.extract(['css','sass'])},
      {test: /\.less$/i, loader: extractLESS.extract(['css','less'])},
      ...
    ]
  },
  plugins: [
    extractCSS,
    extractLESS
  ]
};
```
  
  


  