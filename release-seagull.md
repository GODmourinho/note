
Seagull开发者用的Docker监控管理工具

## 背景介绍

Everyone wants Web UI，这是Seagull开发的初衷和愿景。在Docker发展得如火如荼之际，国内开始有大批开发者进入Docker生态，学习并尝试使用Docker。与众多开发者的交流后发现，被提及次数最多的问题是，有没有好的Docker监控管理工具呢？国外首先兴起Docker，也有部分开源项目实现了Web管理功能，但实用性和易用性都差强人意。

于是Seagull团队要做一个界面友好、基于Web的Docker监控管理工具。项目开发一周后发布首个版本，因易用性得到了开发者的青睐，大家也提了不少新的需求。不到两周在Github上已经超过了100个Star，到现在三周时间又增加了很多新的Feature，实用性和易用性都能达到Docker开发者的期望。

## 项目简介

Seagull是基于Web的Docker监控管理工具，实际上它也是一个Dockerized应用。你可以像运行其他容器那样运行Seagull，例如`docker run -d -p 10086:10086 -v /var/run/docker.sock:/var/run/docker.sock tobegit3hub/seagull`，这样打开浏览器的10086端口，你就可以监控本地的Docker镜像和容器了。

![](https://raw.github.com/tobegit3hub/seagull/master/screenshot.png)

为了让开发者更好地了解Seagull，我们联合Docker中文社区做了三分钟的演示视频，<http://v.youku.com/v_show/id_XODEzOTUzMTgw.html>。如果你只想马上体验Seagull，我们也提供了一个Demo服务器，<http://96.126.127.93:10086>。

## 实现原理

可以说Seagull是轻量级的Docker应用，它的实现原理也很简单，通过Unix Socket访问Docker Remote API然后将数据展示在Web页面上。当然在HTTP服务器的选择，前端框架的实现和多语言的支持上我们做了不少事情。如果一定要列个清单，那么Seagull = Docker + Beego + AngularJS + Godep + Bower + Bootstrap + JQuery.Gritter + Animate.css。

从第一个Commit开始我们就把源代码托管到[Github](https://github.com/tobegit3hub/seagull)上，更棒的是，我们提供了Seagull所有实现细节的中英文文档，你可以挑选你感兴趣的组件来阅读。

* [请求Docker远程API](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-12-access-docker-remote-api-zh.md)
* [海鸥的设计与实现](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-14-seagull-design-and-implement-zh.md)
* [使用Angular-translate实现国际化](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-18-implement-i18n-with-angular-translate-zh.md)
* [海鸥的Dockerfile](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-20-seagull-dockerfile-zh.md)
* [使用Godep管理依赖](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-21-use-godep-to-manage-dependency-zh.md)
* [在Angular实现搜索](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-22-implement-search-in-angular-zh.md)
* [使用Beego为Web服务器](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-23-use-beego-as-web-server-zh.md)
* [使用Beego为API服务器](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-24-use-beego-as-api-server-zh.md)
* [使用Angular](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-25-use-angular-zh.md)
* [使用Bower管理依赖](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-26-use-bower-to-manage-dependency-zh.md)
* [海鸥的Bootstrap使用](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-27-how-seagull-use-bootstrap-zh.md)
* [使用JQuery.gritter实现通知](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-28-use-jquerygritter-for-notification-zh.md)
* [使用Animate.css](https://github.com/tobegit3hub/seagull/blob/master/docs/2014-10-30-use-animate-css-zh.md)

## Roadmap

Seagull 1.0发布以后，得到很多开发者的支持，他们也提出了自己的想法和需求，Seagull接下来会逐步满足大家的需求，成为Docker开发者实用、易用、好用的监控管理工具。

接下来Seagull会着力解决容器资源的监控问题，也希望通过增加Web控制台工具，方便开发者在浏览器做任意的运维工作。同时Seagull会逐步加入对法语和德语的国际化支持，功能也会更加强大，欢迎大家前来我们的Github页面提Issue、发Pull-Request，项目地址 <https://github.com/tobegit3hub/seagull>。