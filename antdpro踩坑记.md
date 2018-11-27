---
layout: post
title: antdpro踩坑记
tags: antd-pro
---


### 如何获取到model中的数据
问题具体描述： react中只有在render中的数据才能同步更新，在函数中无法同步更新会带来不少麻烦，这时我们有可能需要得到及时更新的数据来做些具体的事情
```
// 解决方法
dispatch().then(res => 做自己的事)
// 此处的res事没有数据的，我们需要在model中return出需要的值也就是期望的res的值，则此处即可接受到值
```
### antd-mobile中ListView使用
该组件的datasource需要在本地state中初始化并使用，所以会出现数据和model中数据不同步的问题，需要在dispatch的时候返回数据并且同步保存在model中。
### dva使用注意事项
每个页面都要能做到相互独立尽量做到不受上级页面影响，因为model中的数据再页面刷新时会重置，所以尽量在不要想成页面上的相互依赖，以免用户刷新页面时出现不可预估的错误进而影响体验