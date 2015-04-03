# Mesos In Five Minutes

Mesos是一个简单的资源调度系统，资源调度也就是根据当前服务器的资源使用情况把Job分配到特定的服务器。

Mesos提出了Resource offer的概念，一个offer就是一些资源情况，例如目前剩余2个CPU、2G内存等。

Mesos的架构就很简单了，每台机器都有Agent在收集CPU、内存等使用情况，然后汇报给Master。

Mesos是Master/Slave架构，Master本身是无状态的，它依赖ZooKeeper实现高可用，如果挂了ZK选主产生新的Master。

Mesos Master收集Agent汇报的resource offer，然后转发给上层框架，由上层框架决定是否把Job交给Mesos调度到该机器。

所以Mesos只做资源调度，实际的Job调度由Hadoop、Spark这样的框架确定，确保的灵活性。

Mesos的缺点是它不能保证看到整个集群的资源空余情况，存在Master使用锁算法低效，很难调度对资源要求严格的Job。

Mesos主要是C++实现的，目前支持绝大部分分布式计算框架，如Hadoop、MPI、Spark，还有Marathon等。

Mesos论文地址 <http://people.csail.mit.edu/matei/papers/2011/nsdi_mesos.pdf>
