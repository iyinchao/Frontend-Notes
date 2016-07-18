# Webpack Notes

## webpack.config.js
在运行webpack的时候将自动读取该配置文件中的定义
```javascript
module.exports = {
  entry: './entry.js',   //required,
  output: {       //required
    filename: 'bundle.js'
  },
  devtool: 'source-map'
}
```

## loader
将特定的源代码类型转换成Webpack可以接受的模块/形式。
例如，使用css-loader来将css文件以模块形式载入，style-loader 可以将webpack中的css变成 style 标签插入页面中
安装:
```shell
npm i css-loader style-loader --save-dev
```
使用:
1. 使用管道形式，从右向左传递
```javascript
require('style!css!<path to .css>')
```
这将直接导致css被插入到包含该js的html页面中

2. 使用webpack.config.js


## source map

*TODO: source map 定义*
```shell
webpack -devtool source-map <entry.js> <output.js>
```
build后在chrome devtool中的sources中能够看到一个名为webpack://的路径，能够看到编译前的文件结构，便于debug

```javascript
debugger;
```
用来开启debug断点
