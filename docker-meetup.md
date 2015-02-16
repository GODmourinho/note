# Docker meetup

## Docker

* Become DJAB member
* Virtrualization overhead is 5% and container is 3%
* LXC is not efficient for Cloudfoundry, so waden
* Docker is nice for private PAAS
* Jekins CI uses docker to build 
* Docker cn implements the Drop-like CI
* Docker is used to continously deployment
* Run Hadoop and Spark and the docker-based processing products
* Root roboot to send dbus(not systemd) network will reboot all
* Namespace is not ready for user(root), network card and device
* Paas uses quota(a tool?) for each user to run and pre-allocate 100 users and `tc`
* Store the container in the udisk to run in the host
* Docker doesn't virtualize the resource
* Huawei cgroup maintainer
* Aufs will pull the file from the base fs which is copy-on-write. One user has its writable layer. Here's a problem if pull large files and write from read-only layer
* Hub index will tar the container's files with https
* Docker.cn's docker will compress the files with ace code
* Bug, docker hub private images are stored in the cdn with it's id
* Docker hub use python tool to auth and pull from cdn
* Docker.cn use Ledis DB in go and Chinese
* You may add wget binary and run from Dockerfile which is also trusted
* Almost all do from scatch

## OpenStack

* Nova, Glance, Swift, Heat for ochestation
* Google Omega
* Run container in docker and virtual machines
* Nova to schedule docker as new hypervisor
* devstack supports docker
* Heart plugin to call docker api

## Paas and Iaas

* GAE, sanbox(python interpreter)
* SAE, lxc
* Docker now is good for dev/ops but not for replcing all producation applications
* xdocker for desktop development
* It's about security and network.
* Network OpenSwitch and bridge 
* FreeBSD jail to set address of other hosts, using layer three(NAT) and ARP(latency)
* Docker binds to the seperate network card and have best performance



