# MySQL from Dangming

## Introduction

* 现在的trove数据库没有开binlog
* 一开始就开始
* 可以通过插件支持同步replication

## 优点

* 不影响主的服务


## 缺点

* 一开始就要有主从
* 快照会影响性能，如果不要copy on write数据量很大
* 得控制binlog的保留时间，保留7天


## Work

* 所有实例需要开启binlog
* binlog放在volume上
* 一次创建多个从，或者主压力比较大，可以延时做sync
* 新的slave是从第一个slave同步还是从master同步
* 保证第一个从不能被删
* 备份策略，保存多少天
* 起从的时候就设为read-only
* 从备份可以起一个新的一主一从


## HA

* 通过共享卷起两台虚拟机，主挂了从的数据库再起来
* core sync可以keepalive，可以插件起主从