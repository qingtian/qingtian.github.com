---
layout: post
title: XMPP 高级编程(使用javascript和jQuery) 之入门连接篇
subtitle:   ""
date:       2012-09-21
author:     "qingtian"
catalog:    true
tags:
    - xmpp
---

前几天在博库里逛发现的这本书《Professional XMPP Programming with JavaScript and jQuery》（中文名：XMPP高级编程——使用Javascript和jQuery）由Jack Moffitt著（杨明军译）。

很早之前（大概是几年前吧）就萌生了使用JS写一个WEBIM 的想法，发现网上有很多开源项目，有直接模仿QQ界面来做的，也有单纯做类似Facebook聊天工具栏的。而我的目标就是要做一个基于Web实现的聊天客户端（使用HTML，css,javascript）来实现。

按照书的流程看，一步一步的对XMPP协议有了更深的了解。书的第一部分主要讲的是XMPP协议和架构，内容涉及“了解XMPP协议”和“设计XMPP应用程序”两章。浅显易懂，不断的与HTTP协议对比讲解，更深化了XMPP的优缺点。

XMPP协议主要有以下几个节：presence, message, IQ, error 。而连接生命周期也只有四个阶段：连接，流的建立，身份验证，连接断开。

由于HTTP不支持XMPP协议，于是就有了BOSH (点击[这里](http://xmpp.org/about-xmpp/technology-overview/bosh/)了解BOSH )。通过使用BOSH可以通过HTTP绑定来建立XMPP的支持。本人并未使用书中使用的公共地址，而是自己在本地架设的Openfire服务端，另外使用apache2.2来代理Openfire自带的http绑定地址，前端使用flXHR解决JS跨域问题，使用Strophe来构建各个XMPP节进行数据发送和响应处理。

目前只完成了基本的从网页使用JID和密码来连接本地的Openfire的功能，能正常建立连接和断开连接。但发现无法处理ping请求，我猜想原因在于Openfire不支持ping特性所致。ping不是我关注的重点，我关心的是是否可以正常建立连接和关闭连接，进而是否可以实现最终的一对一聊天，到达这一步就够了，也算起了个好头。一切静待之后的书本阅读和实践情况。

为了更好的实践，我特地在GitHub建了个代码库[webim](https://github.com/qingtian/webim)来发布每一阶段的可用成果，提供一个持续迭代的环境，目标只有一个：“为各个网站集成XMPP协议支持的点对点聊天功能”。

以下列几个所用到的工具地址：

- [jQuery1.8.2](http://jquery.com/)

- [strophejs-1.0.2](http://strophe.im/strophejs/)

- [flXHR-1.0.6](https://github.com/flensed/flXHR/tree/master/code/releases)

- [strophe.flxhr.js](http://code.google.com/p/openfire-websockets/source/browse/trunk/plugin/ofchat/js/strophejs/plugins/strophe.flxhr.js?r=7)

- [openfire_3_7_1.exe](http://www.igniterealtime.org/downloads/download-landing.jsp?file=openfire/openfire_3_7_1.exe)

- [spark](http://www.igniterealtime.org/downloads/download-landing.jsp?file=spark/spark_2_6_3.exe)

参考文章：

[使用 XMPP 构建一个基于 web 的通知工具](http://www.ibm.com/developerworks/cn/xml/tutorials/x-realtimeXMPPtut/section4.html)