# Ceph china community

## CephFS

* NAS is necessary for tradictional business
* AWS EFS(Elastics FileSystem) is under beta
* 动态子路分区，任意热点目录分给任意mds
* CephFS超过40多种锁

## Ceph history

* Storage ceph snapshot into object storage
* Use SSD in any way, but not PCIE

## Ceph operation

* Edit crushmap to add osd

## Ceph IO

* Many bugs in krbd which is maintained by kernel
* Hadoop and spark use libcephfs
* librados natively support aio and callback
* Multiple layers to add overhead
* Apache thread model to async message
* Gluster needs clients to implement transation
* FileStore, kVStore, MemStore(for test, ec), NewStore(SSD double write)
* Theads(by os) to polling
* simple message limits scale(within 30PB), almost 50% CPU switch
* FileStore is the limit
* HDD could be the limit when async data


