---
layout: default
title: 返回顶部兼容火狐浏览
---
# 返回顶部兼容火狐浏览器
## 获取或直接设定当前页面滚动高度：
```js
$(document).scrollTop();//获取，兼容火狐谷歌
```
> 例：
> ```js
> $(document).mousemove(function() {
>     if($(document).scrollTop() < 200) {
>         $('.back-top-btn').fadeOut()
>     }
>     else {
>         $('.back-top-btn').fadeIn()
>     }
> })
> ```

## 有动画效果的设定当前页面滚动高度：
```js
$("body,html").animate({ scrollTop: ... });//动画滚动效果，兼容火狐谷歌
```
> 例：
> ```js
> $(".back-top-btn").on('click',function() {
>     $('html,body').animate({"scrollTop":0},500)
> })
> ```
