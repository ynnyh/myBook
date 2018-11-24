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