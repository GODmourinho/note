
# Day One

## Market place

* The ssd with cpu and memory for ceph cluster. 东芝.

## Keynote 

* The host is hummorous and should be.

## Share to use docker and openstack

* Use slack boto to deploy and query data. Import things.


## The first sponsor

The english from Japenese sucks and it really matters. 


## Answer questions

* Know about the osd deployment


# Day Two

## Keynote

* Use docker for book's examples and make it super easy. getcarina.com.
* We use more serivces than most of others. But they contribute a lot than us.
* Slogo too long.

## Mangum work session

* hot template doesn't support condition and we want to use jinjia
* feedback and patch from TrippO.
* jinjia is simple but add another layer.
* Support both jinjia and hot. But how to make sure the compatiblity.

## Sahara
t
* Different volume for HDFS and internal data result
* optimize with NUMA and socket affinty
* BDD(blockd device driver) of cinder to use disk and reduce the cost of virtualization

## Neutron and container

* Performance test from neutron and kubernetes.
* Double overlay for vm and container cosets low throughtput and high latency.
* The linux bridge just reduces performance.
* Kuryr provides new typoe or neutron port to avoid overlays on top of overlays

## High performance solid state ceph

* Ceph average is good but 99.5% is bad.
* Give the approch to test and mesture the performance of ceph
* Make sure the message is not the botttle-neck and use async message?
* The flash make the bottle-neck to the CPU
* Use all SSD for ceph
* The strage of ssd and hdd is quite differenet, like WAL, change the looging works
* Use mix of ssd and hdd depends on your workload. But all ssd for performance.

## Trove work session

* Support ceph rgw to choose 
* Dont' allow end user to choose where to dump, just for operator
* Learn more about the implemetation of ceph
* Use copy-paste because it's not like guestagent
* Currently don't do anytime

* Snapshot or distributed snapshot, and make the abstraction
* Ask cinder guys for group backup, in cassandra datastore
* Different datastore has different stragy, consistent view
* I may contribute our work for snapshot


* guestagent upgrade
* datastore upgrade for security

* use the key to login, public key and private key
* The users tell you to restart 
* Who creates the instance shoule control them, the end user should not care about that
* We should not care of mysql 5.5 or 5.6, right
* Who's the trigger, The user say, the operator do, should we provide tools 
* Just the security problem of guestgent and datastore, not upgrade 5.6 to 5.7
* The end user need to know the security problem

* How to upgrade ansible, mysql or os? 
* Provide the API to do whatever. Any other will implement by themselves. But we need only one place to insert.
* If sshd down, we can rpc to call.

* The Chinese from Tesora is working on oracle guest image. The other code is open source.


# Day Three

## Magnum and Senlin

* Use senlin for any resource
* Senlin make auto-scalling better than heat, but not in production envrionment
* Heat for provision and Senlin has more policies

## Murano

* Murano uses openstack api and orchestration api from PTL
* Currently murano doesn't support versioning and aplications are not compatible.

## Trove work session for review the patch

* Refactor the code and fix the bug of status at the same time. Make them sperate issues commits so that we can cherry-pick. Otherwise we have check the logic of all datastore.
* For every datstore needs to check all the status before
* It could be better to have the same image for all datastore

It's really wrong when we don't use heat and not follow the community.

* pre_pare runs in pre_pare which we should rename it.

## Kolla

* Excuted exception free, more exception is hell
* Setup ceph the openstack cluster in 11 mins
* Not support murano yet.
* Kubernetes doesn't have pid namespace and net namespace for deployment.
* Relay on docker and ansible.

## Trove work session for build guestagent

* Many problems when the first time to use trove
* We need the devlopment image and production for integration test
* Sahara builds the images in two ways, on the fly
* Run the element script the download the specified version of guestagent
* Sahara has the base image and re-patch it
* The trove image is not that complicated according to the elements
* No matter what you do should be ci tested.

## Ceph and OpenStack

* Trun of debug log to improve performance, maybe turn off by default
* Support swift expiration API now
* More features in mitaka 


# Day four

* Currently we can only use one stragy to support mysql replication
* Some versions of this datastore don't support binlog but others do
* We should implement the stragy per datasotre version before allowing for end users
* Now most configuration is per database but we can that per datastore version
* Associate flavor with datastore version, and all of these can't change
* configuration of mysql 5.5 is different from 5.6 but a lot of same. configuration stragy per version.
* Guestagent in container, the guestagent should run in the same host of database, the problem of how to mount the volume
* We can use nova-docker to have a test, it's deprecated or not 
* We can run guestagent and database in the same container. And run sperately so we can inject the ssh to solve the safety problem with many benific. Move guestagent to host level.
* Move it to next cycle. Make sure we thing it right before changing everything. Upgrade process is really hard.
* Two week or four weeks for integration test
* We should catch up the upstram and upgrade at least once per cycle
* Proposal again and add bp for all-tenant API
* It could be glad for all of us to see the patches
* We don't test horizon for trove
* Make our thing movable from Horizon and just move to the new repo
* Need another core to maintain the horizon part
* Horizon has new generation ui
* redis doesn't support user-create. we should have another way to say we don't support yet or never support that.
* Allow one cycle before totally removing it for deprecated APIs
* add the deprecated tag which means we need to add the gate
* Run two trove clusters to solve the problem.
* Support trove regions.
* Next have the blueprint for them, create someghting and should we do that, descript the problem and the use case
* People want to contribute and no "thanks to your contribution"
* "How do you sleep at night", support kvm in Windows
* File out the bp about quota per datastore
* Ironic and Swift has the tag for stable release 

    