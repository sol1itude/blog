---
title: vue学习遇到的问题
date: 2018-02-26 17:09:44
tags:
---
最近在学习vue，遇到了一些问题，在这里记录一下。
## vue使用路由后刷新页面回到首页
>项目需要在页面刷新后回到首页，使用的是这个方法。这个this.$router.push("/")里面的"/"可以换为你的任何页面，如"/home","/company"。

```javascript
<script>
    export default{
        mounted:function(){
            this.$router.push('/');
        }
    }
</script>
```

>vue2.x---vue-router如何在router-link标签绑定click点击事件、keyup、change等事件。

```<!DOCTYPE html>
<router-link to='/home' @click.native='jump' >page2</router-link>

//在事件后面加native就能解决

```



