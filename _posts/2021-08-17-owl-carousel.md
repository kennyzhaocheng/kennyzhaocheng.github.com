---
layout: post
title: "bootstrap owlcarousel 响应式轮播插件"
description: ""
category: "bootstrap"
tags: ["owlcarousel","css"]
---
{% include JB/setup %}


### Owl Carousel 是一个强大、实用但小巧的 jQuery 幻灯片插件


### Owl Carousel 使用方法：
* html中加入

```html
<link href="css/owl.carousel.css" rel="external nofollow" rel="stylesheet">
<link href="css/owl.theme.css" rel="external nofollow" rel="stylesheet">
<script src="js/jquery.min.js"></script>
<script src="js/owl.carousel.js"></script>
<div id="owl-demo" class="owl-carousel">
     <div>1</div>
     <div>2</div>
     <div>3</div>
     <div>4</div>
     <div>5</div>
     <div>6</div>
     <div>7</div>
     <div>8</div>
</div>
```
以上是加入8个内容轮播，真实情况需替代为需要轮播的内容。例如如下替换为图片，当点击图片时用模态框播放视频

```html
<div name="video2" id="video2" class="item_block wow animated" data-wow-delay=".0s" style="margin: 10px; visibility: visible;">
   <a class="item_link clearfix"></a>
   <!-- <button class="btn btn-default btn-success btn-block" type="button" onclick="VideoPlay()">Video尝试</button> -->
   <a class="item_link clearfix"  data-toggle="modal" data-target="#myModal" data-videourl="./img/videos/02.mp4">
      <div class="item_img">
         
         <div class="media-body" id="media-body-show">
         <img id="Image1" src="./img/videos/02.jpg" alt="video" >
         <div id="video1.1" class="item_mask" >
         </div>
         <div id="video1.2" class="item_ico">
         </div>
         </div>
         
      </div>
   </a>
</div>
```



### JavaScript
```javascript
$(function(){
 $('#owl-demo').owlCarousel({
    smartSpeed: 1000,  //滚动带度
        nav: true, //是还需要导行
        loop:true, //是还循环播放
        navText: ['<span class="iconfont icon-left-2 owl-prev">','</span><span class="iconfont icon-right-2 owl-prev"></span>'],  //左右箭头代码
        responsive: { //跟距屏幕，选择显示多少个视图
            0: {
                items: 2
            },
            450: {
                items: 3
            },
            768: {
                items: 3
            },
            1000: {
                items: 4
            },
            1200: {
                items: 4
            }
        }
 });
});
```

### 参数与api说明

|参数|类型|默认值|说明|
|:--|:--|:--|:--|
|items|整数|5|幻灯片每页可见个数|
|itemsDesktop|数组|[1199,4]|设置浏览器宽度和幻灯片可见个数，格式为[X,Y]，X 为浏览器宽度，Y 为可见个数，如[1199,4]就是如果浏览器宽度小于1199，每页显示 4 张，此参数主要用于响应式设计。也可以使用 false|
|itemsDesktopSmall|数组|[979,3]|同上|
|itemsTablet|数组|[768,2]|同上|
|itemsTabletSmall|数组|False|同上，默认为 false|
|itemsMobile|数组|[479,1]|同上|
|itemsCustom|数组|False||
|singleItem|布尔值|False|是否只显示一张|
|itemsScaleUp|布尔值|False||
|slideSpeed|整数|200|幻灯片切换速度，以毫秒为单位|
|paginationSpeed|整数|800|分页切换速度，以毫秒为单位|
|rewindSpeed|整数|1000|重回速度，以毫秒为单位|
|autoPlay|布尔值/整数|False|自动播放，可选布尔值或整数，若使用整数，如 3000，表示 3 秒切换一次；若设置为 true，默认 5 秒切换一次|
|stopOnHover|布尔值|False|鼠标悬停停止自动播放|
|navigation|布尔值|False|显示“上一个”、“下一个”|
|navigationText|数组|[“prev”,”next”]|设置“上一个”、“下一个”文字，默认是[“prev”,”next”]|
|rewindNav|布尔值|True|滑动到第一个|
|scrollPerPage|布尔值|False|每页滚动而不是每个项目滚动|
|pagination|布尔值|True|显示分页|
|paginationNumbers|布尔值|False|分页按钮显示数字|
|responsive|布尔值|True||
|responsiveRefreshRate|整数|200|每 200 毫秒检测窗口宽度并做相应的调整，主要用于响应式|
|responsiveBaseWidth|jQuery 选择器|window||
|baseClass|字符串|owl-carousel|添加 CSS，如果不需要，最好不要使用|
|theme|字符串|owl-theme|主题样式，可以自行添加以符合你的要求|
|lazyLoad|布尔值|False|延迟加载|
|lazyFollow|布尔值|True|当使用分页时，如果跨页浏览，将不加载跳过页面的图片，只加载所要显示页面的图片，如果设置为 false，则会加载跳过页面的图片。这是 lazyLoad 的子选项|
|lazyEffect|布尔值/字符串|fade|延迟加载图片的显示效果，默认以 400 毫秒淡入，若为 false 则不使用效果|
|loop|布尔值|False|无限循环|
|autoHeight|布尔值|False|自动使用高度|
|jsonPath|字符串|False|JSON 文件路径|
|jsonSuccess|函数|False|处理自定义 JSON 格式的函数|
|dragBeforeAnimFinish|布尔值|True|忽略过度是否完成（只限拖动）|
|mouseDrag|布尔值|True|关闭/开启鼠标事件|
|margin|整数|0|幻灯片间距|
|touchDrag|布尔值|True|关闭/开启触摸事件|
|addClassActive|布尔值|False|给可见的项目加入 “active” 类|
|transitionStyle|字符串|False|添加 CSS3 过度效果|    

<br/>
   
### owlcrousel 回调函数

|变量|类型|默认值|说明|
|:--|:--|:--|:--|
|beforeUpdate|函数|False|响应之后的回调函数|
|afterUpdate|函数|False|响应之前的回调函数|
|beforeInit|函数|False|初始化之前的回调函数|
|afterInit|函数|False|初始化之后的回调函数|
|beforeMove|函数|False|移动之前的回调函数|
|afterMove|函数|False|移动之后的回调函数|
|afterAction|函数|False|初始化之后的回调函数|
|startDragging|函数|False|拖动的回调函数|
|afterLazyLoad|函数|False|延迟加载之后的回调函数|

<br/>

### owlcarousel自定义事件

|事件|说明|
|:--|:--|
|owl.prev|到上一个|
|owl.next|到下一个|
|owl.play|自动播放，可传递一个参数作为播放速度|
|owl.stop|停止自动播放|
|owl.goTo|跳到第几个|
|owl.jumpTo|不使用动画跳到第几个|


----
参考文章链接：[http://www.51zuso.com/admin/p/550.html](http://www.51zuso.com/admin/p/550.html)   
