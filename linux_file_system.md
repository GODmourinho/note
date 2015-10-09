# Linux File System

## Architecture

system call -> vfs -> ext3 or others -> block device or network

## VFS

File is basic unit and consistents of attributes(acl, also known as metadata) and user data

Use tree to management


## Struct

file_system_type, ext4 or xfs, how to build super block

one block device has only one super block instance

inode consistents of metadata

dentry is the location of file tree, one inode can has multiple dentry(such as hard link)

mount instance is the mount location, can mount more than one time

the <mount, dentry> let each file system has its own view

hash string file name to inode and file struct

## Write

async write, wirte to page addir buffer, let BDI thread to write

sync write, should set sync when open file, wait for sync

透写（o_direct）, more strict, write old then new and clean cache

