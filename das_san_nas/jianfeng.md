# Storage

## History

选数器 to distributed storage

## Disk

Four pieces and eight area

CHS addressing 10, 8, 6

LBA addressing convert address into CHS throght computing

IDE(PATA) and SATA use ATA instruction

SCSI and SAS use another instruction

IDE use parallel 80 lines for 80 bit at the same time, but electronical interrupt

SATA is serial and can not controlled

SCSI is expensive and fast

iSCSI use ethernet to send SCSI introduction

FC(Fabric channel) is another fast network with specified protol

IOPS process requests per second

## Raid

Raid0，条带化，来了数据切几份，读写可以并行

Raid1，镜像，解决坏盘问题

Raid3，单节点奇偶校验（存奇数还是偶数个1），在Raid0加一块盘

Raid5，把校验盘分散到多个

Raid6，加两个校验盘，可以坏两个盘

## Architecture

### DAS

Connect disk with server directory

SCSI support 16 disks

## SAN

Middleware use brandnew specified network(HVA?FC?) to connect disks

New switch and network card cost a lot

Use ethernet as transport layer which is iSCSI

Ini(client) and targe(server) will encry and decrypt packages

Problem is the disk can't not be shared

Provides a disk, users need to make file system

Oracle database write raw disk with file system

## NAS

NFS\CIB use FS with network 

Combine disk and file system





