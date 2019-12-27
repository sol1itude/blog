---
title: 在vue使用rem实现移动端自适配
date: 2018-08-31 16:13:21
tags:
---
使用vue.js搭建一个移动端项目，怎样做到自适应呢？当然选择rem布局是比较方便快捷的。之前项目中是使用的淘宝的flexible方案，在vue中也可以继续使用这种方法。
# 1.下载安装lib-flexible
如果使用的是vue-cli+webpack构建的项目，可以直接使用npm来安装
```
 npm i lib-flexible --save
 ```

# 2.引入lib-flexible
在main.js中引入lib-flexible
```
import 'lib-flexible/flexible'
```

# 3.设置meta标签
在项目根目录的index.html 头部加入手机端适配的meta的代码
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

# 4.安装px2rem-loader
在实际的开发中，使用flexible插件时 会自动把px转换成rem单位。在vue-cli中安装过lib-flexible之后 ，将px转换成rem，我们将使用px2rem这个工具。
```
npm install px2rem-loader
```
# 5.配置px2rem-loader
在vue-cli生成的webpack 配置中，vue-loader 的options和其他样式文件loader 最终都是由build/untils.js里的一个方法生成的。所以我们打开build/untils.js文件找到cssLoader，在cssLoader后面添加px2remLoader，如下：
```javascript
const cssLoader = {
    loader: 'css-loader',
    options: {
      minimize: process.env.NODE_ENV === 'production',
      sourceMap: options.sourceMap
    }
  }
const px2remLoader = {
    loader: 'px2rem-loader',
    options: {
        remUnit: 75
    }
}
```
然后找到generateLoaders 方法，在方法里面的loaders数组中添加刚才写的px2remLoader，如下：
```javascript
function generateLoaders (loader, loaderOptions) {
    const loaders = [cssLoader,px2remLoader, postcssLoader]
    if (loader) {
      loaders.push({
        loader: loader + '-loader',
        options: Object.assign({}, loaderOptions, {
          sourceMap: options.sourceMap
        })
      })
    }
}
```
# 6.重新运行项目就可以使用了
直接在项目里写px页面就会编译成rem；这样直接就可以按设计图的尺寸写布局了，如果某个元素不想使用rem，直接在后面写/*no*/就可以，如下
```css
div{
    width: 100px;/*no*/
}
```