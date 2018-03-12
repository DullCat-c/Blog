---
layout:     post
title:      "开发自己的composer包"
subtitle:   "逼格提升之利器,了解composer如何工作"
date:       2018-2-6
author:     "DullCat"
header-img: "https://upload-images.jianshu.io/upload_images/10052831-74fa0d8425062e3a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"
tags:
    PHP
---

>本猫最近准备开源一个php参数验证的包[validate](https://github.com/DullCat-c/Validate),结果一不小心踩入坑中,
特写此博客,以告后者.

- ###### 创建一个仓库
![create_repo](https://upload-images.jianshu.io/upload_images/10052831-1e0cf60a7515ab35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ###### 有一个能分享的代码,并且规范他
你可以按照自己的命名,但是为了便于讲解,我先贴出较为常见的结构:
```
-src/(主要的代码)
--src/Validate.php
--src/Rule.php
--src/....
-README.md (使用说明)
-.gitignore (git上传忽略文件列表,注意加上(.idea/*)编辑器生成目录)
-composer.json(包的说明)
```

- ###### 在本地测试代码是否按照预期执行
如果你想要自己的代码能够被他人正常使用,这一步将非常关键.

1.将包放进vendor目录里

2.在项目根目录的composer.json中写入

```
    "autoload": {
        "psr-4": {
            "Validate\\": "vendor/Validate/src"
             //("命名空间\\":"路径")
        }
    }
```
坑:本地测试要加`vendor`,大小写一定要注意

3.执行`composer update`

然后在本地测试能否正常加载类,如果能正常加载说明你的包可以被使用了
- ###### composer.json
以我的composer.json举例,包里的composer.json非常重要,他不仅是对包的一个描述,
也是composer收录的一个根据

你可以自己直接创建并写入也可以使用命令来创建和写入`composer init`,这里推荐自己写入,因为自由度更高
```
{
    "name": "dullcat-c/validate",
     //"供应商/项目名",如果是个人,可以填自己github的名字,记得要全部小写
    "description": "this is a simple library for php data validate", //简介
    "keywords":["validate","filter","php"], //关键词,记得要用数组形式
    "require": { //需要的依赖
        "php": ">=5.3.0"
    },
    "license": "MIT", //证书
    "authors": [ //作者
        {
            "name": "DullCat-c",
            "email": "cat_cxy@qq.com"
        }
    ],
    "autoload": { //加载的协议,注意这里与本地加载时不一样
        "psr-4": {
            "Validate\\": "src/"
        }
    }
}

```

写完之后记得执行`composer validate composer.json`来检查一下

- ###### 提交给packagist
1.你要拥有一个[packagist](https://packagist.org/)的账号

2.复制你的存储库URL

![vcs_repo](https://upload-images.jianshu.io/upload_images/10052831-247582aa30526e6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.submit你的代码
![submit](https://upload-images.jianshu.io/upload_images/10052831-0ca5258715e14f4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ###### 设置自动更新
1.去到你github项目的主页
2.点击"Settings"
3.点击"Integrations & services"
4.点击"add service",并添加"Packagist"
5.填写
![hook](https://upload-images.jianshu.io/upload_images/10052831-467fea23d702723a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![token](https://upload-images.jianshu.io/upload_images/10052831-af260a7b8174db40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.检查Packagist是否正常显示,如过没有,说明设置失败,请检查上述步骤
![ok](https://upload-images.jianshu.io/upload_images/10052831-44dc27e4b926e2bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ###### 测试是否能使用composer安装
![install](https://upload-images.jianshu.io/upload_images/10052831-01cf5944c0238a58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

安装的目录为
```
-composer.json里填写的供应商名
--项目名
---项目文件
---项目文件
```
坑:版本非常低的php可能在安装的时候需要在composer命令后加上分支名
ex:`composer require dullcat-c/validate dev-master`
