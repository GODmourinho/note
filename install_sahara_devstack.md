## Install sahara devstack

## Setup vm

Start ubuntu 14.04 in linode.

ssh root@172.16.0.206

apt-get update -y
apt-get install -y git
apt-get install -y vim

## Add user

git clone https://github.com/openstack-dev/devstack
./devstack/tools/create-stack-user.sh
su stack

## Configure devstack

git clone https://github.com/openstack-dev/devstack

cd devstack

vim local.conf

```
[[local|localrc]]
ADMIN_PASSWORD=nova
MYSQL_PASSWORD=nova
RABBIT_PASSWORD=nova
SERVICE_PASSWORD=$ADMIN_PASSWORD
SERVICE_TOKEN=nova

# Enable Swift
enable_service s-proxy s-object s-container s-account

SWIFT_HASH=66a3d6b56c1f479c8b4e70ab5c2000f5
SWIFT_REPLICAS=1
SWIFT_DATA_DIR=$DEST/data

# Force checkout prerequisites
# FORCE_PREREQ=1

# keystone is now configured by default to use PKI as the token format which produces huge tokens.
# set UUID as keystone token format which is much shorter and easier to work with.
KEYSTONE_TOKEN_FORMAT=UUID

# Change the FLOATING_RANGE to whatever IPs VM is working in.
# In NAT mode it is subnet VMware Fusion provides, in bridged mode it is your local network.
# But only use the top end of the network by using a /27 and starting at the 224 octet.
FLOATING_RANGE=192.168.55.224/27

# Enable logging
SCREEN_LOGDIR=$DEST/logs/screen

# Set ``OFFLINE`` to ``True`` to configure ``stack.sh`` to run cleanly without
# Internet access. ``stack.sh`` must have been previously run with Internet
# access to install prerequisites and fetch repositories.
# OFFLINE=True

# Enable sahara
enable_plugin sahara git://git.openstack.org/openstack/sahara
```

## Start installation

./stack.sh

```
This is your host IP address: 45.79.183.77
This is your host IPv6 address: 2600:3c03::f03c:91ff:fe3b:9ebd
Horizon is now available at http://45.79.183.77/dashboard
Keystone is serving at http://45.79.183.77:5000/
The default users are: admin and demo
The password: nova
2015-09-26 00:26:23.105 | stack.sh completed in 549 seconds.
```


