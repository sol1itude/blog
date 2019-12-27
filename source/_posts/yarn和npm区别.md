---
title: yarn和npm区别
date: 2019-12-27 11:10:57
tags:
---

下面列举了一些yarn和npm在日常使用中的一些区别

| 对比        | yarn    |  npm   |
| :--------:    | :-----: | :----:  |
| 初始化      | yarn init  |   npm init    |
| 安装依赖    | yarn install或者yarn    |   npm install   |
| 新增依赖    | yarn add axios   | npm install axios --save  |
| 删除依赖    | yarn remove axios | npm uninstall axios --save |
| 更新依赖    | yarn upgrade | npm update |
| 全局安装或删除 | yarn global remove vue-cli | npm uninstall vue-cli -g |
| 同时下载多个 | yarn add axios vue-axios | npm install --save axios vue-axios |


