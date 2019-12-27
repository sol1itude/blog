---
layout: vuecli3.0
title: VueCli 3.0使用人适配移动端步骤
date: 2019-12-27 11:00:39
tags:
---

首先需要先安装'lib-flexible'插件，安装成功之后再main.js引入。

```javascript
npm i lib-flexible
//在main.js引入
import 'lib-flexible'
```

安装postcss-px2rem插件

```javascript
npm i postcss-px2rem
```

最后在package.json中配置一下，添加如下代码
```javascript
"postcss": {
    "plugins": {
        "autoprefixer": {},
        "postcss-px2rem": {
            "remUnit": 37.5
        }
    }
}
```

接下来就可以直接在.vue文件中用px做单位了，运行时会自动转化为rem单位。如果有些样式不需要rem，直接把px写成大写PX就行了。

