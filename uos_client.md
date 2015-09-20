
# UOS Client

## Introduction

给企业用户的差异化，不能太互联网化，性能如何高

能确定这就是我们的用户，我们展示技术就能拿下来

如何避短

企业用户特点有很多部门，不是devops，存储有存储规范，有不同人运维，我们需要都照顾好

行业趋势是openstack、ceph，企业特性是绕不过去的

ceph的积累应该用上

对用户架构不要的苛刻，客户选型已经是高密度，只用sata性能也可以接受

## Network

尽可能网络的转发都在物理设备，周期比较短的用vxlan，长期用的IP有用户管理员控制

基础网络的创建和管理有用户操作，用户创建，用户去配置交换机，异构很强不能自动同步

同时管理物理网和虚拟网，卖点

两个网卡做bond，两面两个交换机做SAP冗余（接入冗余），无法接受单点，就多几十万

## Storage

存储放ceph，关键数据放阵列，给出cinder的兼容性列表

如果有设备，网络通了，即可以测试

ceph设计目标不同，有必要支持主流的存储（高端）

阵列一般配以太网，FC也支持

基于交易的transation都是基于整列，对象、文件觉得阵列太贵

定义我们的目标，归档、文件，对外的卖点

是否已经已经有NAS？不是传统的NAS移过来？

## Image

用户镜像上传，用户定制

目前通过快照来做

用户可以接受命令行操作，可以失败，要有灵活性

刻通云等是BUIO，不能改镜像的大小

## Operation

国泰，汽车等大体量，不能托管，自己请外包做监控管理升级

流程比互联网苛刻，有人支持下就好

管理overhead比较高，虚拟机管理物理机，不需要5台，降低平台复杂度

从VMWare过来，一台物理机即可

打散OpenStack的模块，部署的时候用户选择部署的功能

以产品为中心，追求新功能，VMWare只做虚拟机但不会出错

## Module

企业用户有定制型要求，有些功能不需要如内部用的计费，

互联网全托管，有些半托管由用户自己控制

用户可以选择功能，前端都支持

用户拓展功能通过API或者集成用户，像思科的role base，目前只有管理员和观察者

openstack设计好，上游实现不好，aws好

整合业界的工单系统，已经整合了SaaS

## VMWare

产品上的合作

支持双栈，通过API连管理不同的套件，统一的面板

VMWare从来没有支持过开源kvm，CTO通过vio对开源的支持

桌面由他们实现后端，统一的方案在UOS，用户只需要买终端

## Ctrix

目前支持桌面的功能，没有新的功能，换成opestack、分布式存储

uos + ctripx方案，没有vsan，ceph已经实现了thin provision，对存储架构简化很多

别人要看的是完整的解决方案


为华讯建space，分