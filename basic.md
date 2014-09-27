
# Basic

## Infrastructure As A Service

能控制操作系统和存储的。



## Platform As A Service

提供开发工具或运行环境，不需要考虑网络磁盘。

安全问题。

## Software As A Service

提供软件，直接通过浏览器访问。

我们平时的软件就这样，没什么好说的。

## Contain As A Service

有局限性，因为单个Container可能做不了一个Service，而且CaaS不会做Orchestration。

很容易实现，做个服务器接受HTTP请求然后创建Container，写个Web页面展示运行信息（像tutum.io）。

Container内跑个Sshd，然后通过VNC就可以在浏览器登录和学习了。

## Shiyanlou

实验楼用虚拟机，启动速度慢，但资源隔离好，安全性高。

