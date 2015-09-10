# Glance from Mingda

## Introduction

* glance已经支持v3，我们主要是用v2
* glance还有用v1接口来抓list
* v1通过registry来访问db，现在v2不走registry直接走db
* registry主要做参数检查等，v2已经在api做了
* raw是空洞文件，实际不会占这么多
* qcow2是压缩过，1.6G，镜像内header记录元信息，virtual size为20G依次转成raw，还有很多信息有格式
* glanceclient新版支持--file，其实也是create再upload
* 安装ceph时会把python library安装在所有节点
* glance的rbd并不知道raw是空洞文件，20G传太久了，
* 现在我们是使用rbd import来导进去，再通过image update
* 使用rbd命令不管是否空洞文件一定会补全，否则不完整，通过文件系统来读会补全，使用memory compare
* 目前ceph支持最好的raw格式
* 配置文件有个worker，一般跟cpu数对应
* 我们改了代码允许用户传qcow2文件，然后在本地存下载，再转成raw
* 我们通过qemu-img转镜像，需要需求空间来存新的镜像，新版支持直接rbd://就不落本地磁盘
* glance没有快照的概念，全是image
* version negotation可以容错，v1、v1.0、1.0也认为1
* 我们改了数据库来支持icon等属性
* 通过upload接口就要走20G，如果rbd命令行就有memory compare



## Question

* rbd导入20G的raw文件，会过滤空洞部分，ceph显示20G，实际没有20G，但不影响实际使用
* 为什么目前ceph支持raw最好
* 我们的镜像都会打个快照，不知道为什么
* 通过rbd命令默认按8M存，但ceph系统式16M，我们传的时候不能指定16M，rbd -p $pool info $image(order设为24)
* ceph.conf没有指定16M，rbd指定这个文件不知道，默认用8M
* A生成一个快照B，基于B生成C，删除A报错并失败，删除C，剩下B就多余删不掉了，数据库也没有了
* nova打快照与glance的逻辑，nova用了qume是完整的，这只是file后端，ceph肯定不是这样
* member是可以分享给别人的
* cache不是缓存镜像实例