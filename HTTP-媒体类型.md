---
layout: post
title: HTTP-媒体类型
tags: HTTP
date: 2018-10-19
---


### 媒体类型
Web服务器会为所有HTTP对象附加一个MIME类型。当Web浏览器从服务器取回一个对象时，会去查看相关的MIME类型，看看它是否知道如何处理这个对象。
MIME类型是一种特定的文本标记，表示一种主要的对象类型和一个特定的子类型，中间由一条斜杠分隔。
* HTML格式的文本文档由*text/html*类型来标记。
* 普通的ASCII文本文档由*text/plain*类型来标记。
* JPEG格式的图片为*image/jpeg*类型。
* GIF格式的图片为*image/gif*类型。