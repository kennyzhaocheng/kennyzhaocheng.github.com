---
layout: post
title: "用Bootstrap Modal模拟框播放视频"
description: ""
category: "Bootstrap"
tags: ["Modal","css"]
---
{% include JB/setup %}


### 什么是Modal
模态框（Modal）是覆盖在父窗体上的子窗体。通常，目的是显示来自一个单独的源的内容，可以在不离开父窗体的情况下有一些互动。子窗体可提供信息、交互等。
如果想要单独引用该插件的功能，那么您需要引用 modal.js。或者，您可以引用 bootstrap.js 或压缩版的 bootstrap.min.js

### 引用Modal

```html
<a class="item_link clearfix"  data-toggle="modal" data-target="#myModal">
    <div class="item_img">
        <div class="media-body" id="media-body-show">
            <img id="Image1" src="./img/videos/01.jpg" alt="video" >
        <div id="video1.1" class="item_mask"> </div>
        <div id="video1.2" class="item_ico"></div>
        </div>
    </div>
</a>
```
* **data-toggle**: HTML5 自定义的 data 属性, data-toggle 用于打开模态窗口。
* **data-target**: 是要在页面上加载的模态框的目标。

### Modal 窗口

```html
    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <button type="button" class="close" data-dismiss="modal" aria-hidden="true">x </button>
                <h4 class="modal-title" id="myModalLabel">
                Video
                </h4>
            </div>
            <div class="modal-body">
                <video controls="" name="media" draggable="true" id="videoModal1"><source src="./img/videos/01.mp4" type="video/mp4">
                </video>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-default" data-dismiss="modal">Close
                </button>
                
            </div>
        </div>
    </div>
    </div>
```
* **class="modal fade"**: 
   1. .modal，用来把 div 的内容识别为模态框
   2. .fade, 当模态框被切换时，它会引起内容淡入淡出。
* **id="myModal"**: Modal Id。 模态框加载的目标。
* **tabindex="-1"**： 
* **aria-labelledby="myModalLabel"**： 该属性引用模态框的标题。与下面的<h4 class="modal-title" id="myModalLabel">相呼应。
* **aria-hidden="true"** 用于保持模态窗口不可见，直到触发器被触发为止（比如点击在相关的按钮上）。
* **class="modal-header"** 是为模态窗口的头部定义样式的类
* **class="close"** 用于为模态窗口的关闭按钮设置样式
* **data-dismiss="modal"** 是一个自定义的 HTML5 data 属性。在这里它被用于关闭模态窗口。
* **class="modal-body"**，是 Bootstrap CSS 的一个 CSS class，用于为模态窗口的主体设置样式
* **class="modal-footer"**，是 Bootstrap CSS 的一个 CSS class，用于为模态窗口的底部设置样式

### 加入必要的js
```html
<script>
    $(function () { $('#myModal').modal('hide')
    });
</script>
  
<script>
    $(function () { $('#myModal').on('hide.bs.modal', function () {
      var myVideo=document.getElementById("videoModal1")
       myVideo.pause();
      })
    });
</script>

<script>
    $(function () { $('#myModal').on('show.bs.modal', function () {
      var myVideo=document.getElementById("videoModal1")
       myVideo.play();
      })
    });
</script>
```
* ***hide.bs.modal***: 当调用 hide 实例方法时触发。
* ***show.bs.modal***: 在调用 show 方法后触发。

### 引用必要的css,js
```html
<link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/css/bootstrap.min.css">
<script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
<script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>
```