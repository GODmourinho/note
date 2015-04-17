# Sohu PaaS

## LXC

LXC+MQ+OVS，架构呢？OpenCV？三层隔离？

MQ

OVS（OpenSwitch），Network isolation

UTS Namespace，hostname、domain name

NET Namespace

## Problem

容器内进程异常退出监控。一个container多个进程？

容器网络流量控制，根据classid用tc实现。

Proc文件系统隔离，top错。使用/proc/sysrq-trigger重启。

container和host共享同一个cgroups。

使用setns()。

使用overlayfs。

支持netoops。Google搜集远程资源信息，对container。

## 云景

可用性，冗余N

伸缩性，利用规则引擎，实例动态调整

Python用requiements.txt，Ruby用gemfile，Node用package.json

配置app.yaml，nginx，php，环境变量

代码最终都系git。

有状态的？放cache，存储。

粘性回话，绑定app cookie、user、container。

监控qps、响应时间、容器性能。

acees、app、stdout日志，日志实时分析系统FlumeNg，Spark、Storm。

all services should bind，动态策略。

外部域名必须备案。

Cache（Memcache多租户，Redis主动）、Storage（分块）、MySQL（主从、phpmyadmin）。没有MQ。

单实例升级会不可用，多实例rolling update。建议多实例。

MySQL proxy进行acl、黑白名单、危险操作。

和Azure合作用Windows虚拟机。

cgroups动态修改配置，不用重启。

ssh提供只读权限。Tomcat配置不能更改。
