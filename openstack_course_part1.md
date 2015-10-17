# OpenStack Course

Glance -> Cinder -> Nova -> Trove

No swift because we use ceph.

## Glance

rest api uses event.wsgi, routes, paste and so on

client -> api server -> db and store

## Wsgi

wsgi is between web server and python code

paste is the advanced ini file to compose applications with pipeline

No web server?


# Ceph Course

## Ceph

The paper ceph and 200 pages

Ceph focus on how data distributed and mange meta data

Ceph FS is not good

OpenStack used LVM(single server) and need distributed storage

Read the paper and how it comes. Important.

RADOS is the tcp level. Wrap as so file as api witch is librados.

Radosgw is rest or http only contains s3 and swift(translate protocol).

RBD is block device. block device dives into 4M pieses for each object(linux kernel and quem).

CephFS depends on MDS server which meets vfs.

librados has both user mode and kernel mode interfaces.

OSD is the process based on fs(btrfs, xfs, ext4).

ext4 is not for big file and meta data. btf is not for production. Now all xfs.

## CRUSH

Define a decision tree. Allow policy to determine data in each node with crush rules and map.

client will compute.

first hash to pg(cpu acceleate), second hash to osd which is logical and stored in map which no need to compute.

raid 5 does XOR, 110 can compute the next.

monitor containers meta datas.

use cgroups to devide virtual machine and disk

fio test performance

glance -> dd in rbd volumn -> make snapshot -> clone image

create an instance = clone an image(copy on write)

scaleio