# Puppet


## Background

* CF Eegine update slowly
* ISConf paper and why it fails
* Anything should crush down to one state

## Introduction

* Declare DSL
* Idomophone

## Usage

* Resource, the unit of puppet including package, file, customed ones
* Use dependency to manage all the resources, all the resources are unorders and crush to one state
* Class is to collect the sources, 
* Module collects the classes, the max unit to reuse, need init.pp file in manifest directory
* Client/Master uses HTTPs
* client sends Facts with environment and generate catelog to client
* Factor will expose all the environment vars
* Write the ruby files to extend factor to get more vars
* Puppet is not for large number of static files
* Puppet doesn't do ssh or ansible job, just configuration management
* Use cluster shell for single node, mco(Java) for cluster
* Ansible to init cloud infra, and do minor upgrade
* Node is the servers with same role 

## Deployment

* 80+ moduels
* one for ustack public cloud, one for private clouds, two for test
* new cloud will add to test, and migrate to shared private cloud
* Puppet advanced feature to oprate automatically when adding new server
* 40 cores should handle less than 40 requests at one time
* Use random time around one hours to check slave
* Hiera store the common data in one place