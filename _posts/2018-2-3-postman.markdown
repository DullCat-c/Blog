---
layout:     post
title:      "Postman使用手册"
subtitle:   "实用的Postman使用方法"
date:       2018-2-3
author:     "DullCat"
header-img: "https://upload-images.jianshu.io/upload_images/10052831-2c7ced673b46ac90.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"
tags:
    Tools
---

>如果你用PHP写web,你可能不需要这个工具,因为你可以直接在视图层直接测试接口是否
达到了你预期的效果.

>但是如果你是写App,或者是前后端完全分离的场景,你就会非常需要这个工具.

>因为在前后端完全分离的场景,你无法借助html和js来发请求,但是你又必须要进行测试,
一方面你无法百分之百保证接口正常运行,一方面是有底气与不看接口文档就说接口错误
的前端撕逼.所以你需要一款测试接口的工具.

>而测试工具那个比较好呢?我开始设想了四个要求:1.及时修改DNS 2.测试接口参数能被复用
3.支持各种参数传递(文件,json) 4.方便简洁

>后来我先后用了Postman,cURL,HTTPie,单元测试,都各有缺陷.Postman无法快速切换DNS,
HTTPie和cURL的参数无法复用,单元测试过于繁琐

>但是综合起来还是Postman比较实用,可以非常方便我们的测试.

>接下来我就介绍一下Postman的常用用法

- ###### 下载
你可以下载谷歌的插件应用,也可以去官网下载原生应用,不过据官方说原生app的效果更好,
并且不需要翻墙.

- ###### Send Request
![send](https://upload-images.jianshu.io/upload_images/10052831-b738d77232af1aae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就这么简单你就可以完成一次接口测试

- ###### Create Collection
![collection](https://upload-images.jianshu.io/upload_images/10052831-c24fc639704d3deb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个功能可以让你的测试无限复用,而不必每次都重新输入参数和url

- ###### Create Environment
![environment](https://upload-images.jianshu.io/upload_images/10052831-875ae1c0f0171dd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![variable](https://upload-images.jianshu.io/upload_images/10052831-ecb6462120a120c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

善于利用环境变量能让你更省事的测试接口


- ###### Code Generate
![code](https://upload-images.jianshu.io/upload_images/10052831-05cf0644c576b254.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为Postman无法快速切换DNS,有时候因为线上线下环境不同而导致的bug,
需要切换DNS来进行测试,所以我们可以利用此功能将接口信息转化为curl代码,
在powershell/cmd中执行向线上发的请求.
如果有小伙伴不知道怎么在windows使用cURL,下面是我分享的百度云盘的下载链接
链接：[百度云盘](https://pan.baidu.com/s/1jJoKWK2) 密码：s80m

- ###### Postman pro
如果你想要保存你所用的接口信息,并且很清晰的让让团队的其他人看到,你可以做一份接口文档.
这也就是postman的收费服务了,支持团队共享接口文档.不过收费颇为昂贵,一个人一年的费用
约等于450人民币.不过你还有很多种选择,比如国内的小幺鸡,如果你不放心第三方服务,你
也可以选择搭建一个自己的接口文档.GitHub上有很多这样的项目.
