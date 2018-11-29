---
layout: post
title: redux翻译 
tags: redux
---


### Motivation （解释这个玩意出现的动机）
    随着JS单页面应用程序的要求开始变得越来越复杂时，我们需要比以往管理更多的state，这个state可以包括服务器响应和缓存数据（This state can include server responses and cached data），以及尚未持久保存到服务器的在本地创建的数据（as well as locally created data that has not yet been persisted to the server（此处需要结合上下文去翻译，因为上一句是在讲存储，所以此处借as well as讲本地数据存储的东东））。UI state的复杂度也在增加，比如我们需要管理激活的路由（active routes）、选中的选项卡、是否显示加载动效（spinners（此处翻译取自官网emmm不理解什么意思大概是在说动效之类的）），分页控制等等。。。
    管理这个不断变化的state是很难的，如果一个model可以更新另一个model，然后一个view（视图）可以更新一个model，这个model又可以更新其它的model，那么反过来说这个model可能会导致另一个view（视图）更新。在一些关键点上（At some point），你不再理解你的app发生了什么，因为你已经失去了对state在时间、原因和方式上的控制。当一个系统是不透明且具有不确定性的时候，它会很难复现bug并且很难去添加一个新的功能。
	这还不是最糟糕的，在前端产品的开发中考虑新需求是很普遍的。作为一个开发者，我们要更新调优（handle optimistic update）、服务端渲染、在路由跳转之前获取数据等等。我们发现我们正在尝试管理此前从未遇到过的复杂性。我们必定会问出一个问题：“是时候放弃了吗？”答案是否定的！
	这种复杂性很难处理，因为我们混合了两个对人类而言非常难以理解的概念：变化和异步（mutation and asynchronicity）。我称呼它们为 Mentos and Coke。它们两个都可以做的很好在分离的时候，但是在一起的时候却又一团糟。像React这样的库试图通过移除异步操作和移除直接操作DOM来解决这个问题。然而，管理这些状态取决于你，这就是Redux进入的地方。
	紧随着Flux、CQRS和Event Sourcing，Redux尝试让状态的变化可以预见，通过对更新发生的时间和方式施加某些限制。这些限制反映在Redux的三个原则中。