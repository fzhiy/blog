---
title: "[Typecho]使用https连接问题"
date: 2020-08-15T20:47:09+08:00
draft: false

toc: true
categories: [blog]
tags: [Typecho]
keywords: []
description: ""
---



# 写在前面

**当前，为了安全大多站长将https应用于网站中，我也不例外。由于个人更关注内容输出，所以希望内容外的东西越少越好，所以博客采用了宝塔面板建立基于Typecho模板的个人博客（此处不再争论用什么方式最佳，每个人心中都有一个哈姆雷特）。**

## 问题描述

​	前面已经提到，使用了宝塔面板，很容易可以在面板中SSL处使用Let's Encrypt证书改用https连接。**不过由于之前站点的头像以http形式调用了gravatar，所以用google浏览器访问网站时还是会显示不安全（你的图像……之类的）。**

具体出现的问题 **则是混合内容：页面已通过 HTTPS 加载，但请求了不安全的图像。此内容也应通过 HTTPS 提供。 **

![](https://img.fzhiy.net/img/20200815134933.png)



## 解决方案

访问时F12打开控制台查看是否有http访问，具体见https://developers.google.com/web/fundamentals/security/prevent-mixed-content/fixing-mixed-content?hl=zh-cn

发现所给的方法需要将混合使用的http手动修改为https， （如果要是不止这一个地方，那得多麻烦，不想动……），于是发现了[Typecho修改gravatar头像源为国内服务器源](https://www.lanka.cn/gravatar_3303.html)。根据第四条，改为国内源即可。

### 方法一

![](https://img.fzhiy.net/img/20200815135258.png)

```php
define('__TYPECHO_GRAVATAR_PREFIX__', 'https://cdn.v2ex.com/gravatar/');
```

### 方法二

用了方法一后我发现 自己还是没利用好主题Initial，其实**后台设置中改成国内源就OK了**，不需要去修改config.inc.php。 

--fzhiy.更新于2020年8月15日13点59分