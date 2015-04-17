
# 十分钟开发HBase应用

## 背景

[HBase](http://hbase.apache.org/)是一个分布式NoSQL数据库，部署HBase集群并不简单，即使是搭建开发环境也有各种坑。虽然官方提供了单机版HBase，但你还需要知道Trunk的代码只能运行在Java7以上，不要企图用OpenJDK，构建项目需要安装Maven，如果不注意这些搭个环境就不止10分钟了，更别提陷入[Dependency Hell](http://en.wikipedia.org/wiki/Dependency_hell)。

有人说过“让Hadoop开发像家庭作业一样简单”，容器技术的出现让这成为可能，可以用Docker封装HBase运行环境，通过统一的接口来运行。本文将介绍如何在十分钟内跑起你的HBase应用。

## 实践

首先，我们需要安装HBase集群来开发和测试，下载HBase源码运行单机版固然可以，前提是你已经安装配置好Java和Maven环境，或者你可以运行命令`docker run -d --net=host tobegit3hub/standalone-hbase-0.94`。

这个命令会下载名为standalone-hbase-0.94的Docker镜像，以Daemon形式运行。具体它做了什么呢？可以查看它的[Dockerfile](https://github.com/tobegit3hub/standalone-hbase-0.94/blob/master/Dockerfile)，原来就是安装Java7、下载HBase源码、然后编译和运行单机版HBase。如果镜像已经下载，只需几秒钟就可以创建干净的HBase开发环境了。

然后，我们可以编写HBase应用了，我已经开发了一个[smoke-hbase](https://github.com/tobegit3hub/smoke-hbase)的程序，它会在本地的HBase集群创建表、插入数据、读数据、删除数据和删除表。怎么运行呢？也是一行命令`docker run -i -t --net=host tobegit3hub/smoke-hbase`。

运行结果如下，不管你在什么系统执行的，Docker可以保证运行结果是Repeatable和Predictable的。

```
➜  standalone-hbase-0.94 git:(master) docker run -i -t --net=host tobegit3hub/smoke-hbase
14/12/03 12:06:11 INFO zookeeper.ZooKeeper: Client environment:zookeeper.version=3.4.5-1392090, built on 09/30/2012 17:52 GMT
14/12/03 12:06:11 INFO zookeeper.ZooKeeper: Client environment:host.name=localhost
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:java.version=1.7.0_72
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:java.vendor=Oracle Corporation
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:java.home=/usr/lib/jvm/java-7-oracle/jre
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:java.library.path=/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:java.io.tmpdir=/tmp
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:java.compiler=<NA>
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:os.name=Linux
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:os.arch=amd64
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:os.version=3.16.5-x86_64-linode46
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:user.name=root
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:user.home=/root
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Client environment:user.dir=/smoke-hbase
14/12/03 12:06:12 INFO zookeeper.ZooKeeper: Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=180000 watcher=hconnection
14/12/03 12:06:12 INFO zookeeper.RecoverableZooKeeper: The identifier of this process is 1@localhost
14/12/03 12:06:12 INFO zookeeper.ClientCnxn: Opening socket connection to server localhost/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
14/12/03 12:06:12 INFO zookeeper.ClientCnxn: Socket connection established to localhost/127.0.0.1:2181, initiating session
14/12/03 12:06:12 INFO zookeeper.ClientCnxn: Session establishment complete on server localhost/127.0.0.1:2181, sessionid = 0x14a10076dad0007, negotiated timeout = 40000
Create table table1
14/12/03 12:06:13 INFO zookeeper.ZooKeeper: Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=180000 watcher=catalogtracker-on-org.apache.hadoop.hbase.client.HConnectionManager$HConnectionImplementation@9bc2c97
14/12/03 12:06:13 INFO zookeeper.ClientCnxn: Opening socket connection to server localhost/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
14/12/03 12:06:13 INFO zookeeper.ClientCnxn: Socket connection established to localhost/127.0.0.1:2181, initiating session
14/12/03 12:06:13 INFO zookeeper.ClientCnxn: Session establishment complete on server localhost/127.0.0.1:2181, sessionid = 0x14a10076dad0008, negotiated timeout = 40000
14/12/03 12:06:13 INFO zookeeper.RecoverableZooKeeper: The identifier of this process is 1@localhost
14/12/03 12:06:13 INFO zookeeper.ClientCnxn: EventThread shut down
14/12/03 12:06:13 INFO zookeeper.ZooKeeper: Session: 0x14a10076dad0008 closed
Put data row1
Get data row1
Get value: value1
Scan table table1
Delete row row1
Drop table table1
14/12/03 12:06:13 INFO client.HBaseAdmin: Started disable of table1
14/12/03 12:06:15 INFO client.HBaseAdmin: Disabled table1
14/12/03 12:06:16 INFO client.HBaseAdmin: Deleted table1
```

不管你对Docker是否感冒，强烈建议运行上面两条命令，你只需安装Docker就可以了。Ubuntu通过`apt-get install docker.io`安装，CentOS可以执行`yum install docker`，Mac用户只要下载[boot2docker](http://boot2docker.io/)即可。

最后还有一个问题，如何修改应用的代码？一种方式是直接进入Docker容器修改，这样就只能在tty(命令行)操作了，另一种方式是使用Docker的volume特性。例如执行`docker run -i -t --net=host --privileged -v /:/host tobegit3hub/jde`，这会创建一个安装好Java7和Maven的开发环境，你使用IDE修改完代码后，Docker容器能直接读本地文件系统的代码，然后在容器内运行你的应用就可以了。

## 总结

有没有发现，要开发HBase应用，甚至连Java都不需要安装了，没有任何依赖，因为Docker已经把Runtime做好了。对开发者来说，只要管代码就好了，开发环境给你做好了，对运维人员来说，只要运行容器就可以了，容器本身没有其他依赖。

十分钟开发HBase应用，让Hadoop开发像家庭作业一样简单，这逐渐成为现实了。当然这还需要你对Hadoop和Docker有基本的了解，容器是个很好的概念“Escape Dependency Hell”，已经广泛应用到Google的生产环境上了，希望大家继续关注:-)
