# puppet openstack

## Introduction

class只能定义一次，define可以定义多次（例如cinder多个backedn）

-> 表示执行后一个之前执行前一个

~> 表示前一个资源发生变化，后一个一定要执行（重启或者stop）

$::osfamily 就是factor， 通过agent那里facter agent获取的值

静态文件用file来管理，动态文件自己写erb模板，还能拼接小模板成大模板，最后自定义resource可以传参数

resource是没有命令空间的，define是有命名空间的


## Study

use all-in-one puppet openstack

read source code of puppet-openstack-integration

basic requirement in http://confluence.ustack.com/pages/viewpage.action?pageId=7374277



