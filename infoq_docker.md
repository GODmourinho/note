# InfoQ Docker

## OpenStack

* 20M+ code
* Easy to use some of components
* Magnum let OpenStack supports containers well

## IBM BlueMix

* Support CloudFoundry, Docker and OpenStack
* Command-line tool(ICE) to warp docker
* Use docker-registry and docker hub
* Support persistent mintor and logger

## DataMan

* Mesos use ZooKeeper for leader election
* Run marathon, chronos(to manage batch jobs), spark and storm
* Mesos allocate resource by providing resource offer(like airport)
* Spark as a service

## Docker Security

* pid cgroup, prevent fork bomb
* namespace/cgoups not enough as virtual machine, and never
* read-only mount, COW, Selinux/apparmor, grspc,seccomp, capabilities
* should support user namespace, run as non-root user

## SkyForm

* Need to schedule resources
* Add path infomation in docekrfile
* Yarn model of node, container and resource
* Focus on application management and performance

## In Production

* Must use Dockerfile to trace issues
* Multiple processes in container should have init system forward SIGTERM
* Use git sha as image tag
* push to gitlab which trigger jenkins to build image and push to registry
* log, syslog #10568

## Qiniu Build

* List the wrong ways
* Use base image for each application, not agreed

## Weibo

* Hierarchiture to reduce image sizes, use dockerignore, abandon latest
* One process in container, volume to reduce device maper IO
* single point docker-registry, use docker-registry-frontend
* host network mode, use ovs + vlanif or socketplane to solve conflict
* Implement service discovery with nginx
* Implement orchestration tool with pod like kubernetes
* Improve cAdvisor for their monitor system

## Snowball

* LXC for stateful applications like MySQL
* Use ubuntu-upstart base image to support sshd, zabbix, cron, logroate
* Add ssh and monitor as script in base image
* Use bridge and remove docker0
* Pass --ip for operation and mac for monitor


