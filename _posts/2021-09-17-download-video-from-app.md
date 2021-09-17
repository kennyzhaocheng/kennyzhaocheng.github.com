---
layout: post
title: "下载手机app中的视频"
description: ""
category: "tools"
tags: ["app","fiddler","video"]
---
{% include JB/setup %}

手机app中有些视频挺好的，比如给孩子讲故事的视频，但是很多需要VIP才能看。有的提供几天VIP试用期,如果可以在这几天VIP试用期里把视频给下载下来，岂不是很好。

app中视频播放一般都是先发送一个请求给到server,然后得到视频的链接或者数据，最后播放出来。

我这里的讲的是server返回来的是一个视频链接的情形，用的Fiddler抓包工具。

### 步骤如下：
* 确保手机和电脑在同一网络内
* 电脑端打开Fiddler, Tools --> Options --> Connection, 
   * Fiddler listening port 设为8888. 
   * check "Allow remote computers to connect". 
* 手机网络设置代理，代理服务器为电脑。 
   * 在手机设置中找到手机在连网络，点进去找到配置代理
   * 代理服务器为电脑IP,端口设为8888
* 打开手机app, 播放视频
* 在电脑端Fiddler 上查找播放视频的请求和反馈。找到视频链接。
* 下载视频。
 
