## Optimize micro packages

一个网卡队列对应一个core，出入也是一个core，借鉴无锁编程

intel支持一个core发一个core出，编号+1，解决了一个core很忙的原因，但是cache miss

改sed后hash可以对称，保证走一个core


以前内核2.6.38， 我们3.12

开超线程，一个core当两个core，提高cpu利用率，超过硬中断个数，服务器核少可以开

batch写cpu寄存器，较少cpu时间，单个请求延时大，保证31个包也能发

有个tc锁，锁住所有core，重写了列用真正无锁编程

两个cpu当一个cpu，cpu有本地内存，两个cpu通过QPI通信，当两台服务器用，必须保证包上这个core处理也要在这个core

固定包的大小，分配大固定间隔的内存，每一个映射到不同DMA接口，读写分离不会同时block DMA和CPU


八G优化到二十几G，没有tc，每个CPU死循环一直100%

namespace设计包拷贝，使用多路由表代替，隔离做不了得改代码

开超线程，L1太小会被刷掉，L3还好

L1 miss损失10个cycle，L2是20-30 L3是50，memory有几百个cycle