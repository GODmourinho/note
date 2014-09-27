
# Kernel

* ceph
* btrFS

## FS

* DirectIO write page cache(radix)
* Block io CFQ IO priority and dieline(io schedule)
* BufferIO low latency brust and tune journal checkpoint 

## Ceph

* Monitors run with paxos, map of status
* OSD to store data
* Consistent hash(Crush algorithm)
* Memory ack and disk ack
* Monitor manages osd's status
* Client has the verson of status to request
* Placement group(compute router table)
* Monitor + OSD(distributed object storage), block device(RBD 4M object), thrift, DFS(CephFS)
* Namespace, meta data 
* XFS(big data serval M), B+ tree to allocate continous file 
* btrFS has so many features
* Network partion
* Seperate large file

## BtrFS

* Snapshot the root node of tree
* BtrFS has multiple roots B+ tree.
* XFS can commit nodes and release resource
* F2FS for SSD in Samesung
* Not only copy on wirte(snow flow), rotate
* Block index to extended index
* Now is for SSD(磨损均衡), Error detection
* /proc/stack
* core stack, function name + 0xxx

## Kernel debug

* vmlinux, module gdb module
* gdb vmlinux
* ls \* function + xx
* build image with source
* kgtp the backtrace line
* trace + perf probe
* maybe disk error
