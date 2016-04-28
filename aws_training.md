# AWS Training

Route 53, 支持延时检测、是否可达、权重轮训的方法来解析


nova也做placement group，还有做互斥的不放在同一个主机中（用不同placement grouop？）




DynamoDB
hash key is partiion key
range key is sort key
support二级索引
case study或购买support


rds主从保证5秒内同步


piops是什么？
使用ebs-optimized instance
（考虑cinder如何实现ebs-optimized instance）
rds如何导入数据？import或export指导？每个数据库都有文档。

Redshift兼容Postgres的数据仓库



CloudTrail只记录API调用，OpenStack没有类似项目，像计费一样实现


RDS默认就有一个隐藏从


CloudFormation支持VisualOps、CloudFormation designer图形化编辑
region中用CloudFormer导出json文件
模板会列出所有修改历史
有policy可以决定创建过程中失败是rollback还是停下troubleshooting
通过参数传到模板中
通过mapping来设置region map，如果是us-east1就选什么
outputs，生成的结果返回json信息
condition，根据条件来创建
DependsOn写依赖关系，指定创建时间
在json里获取某个变量，然后传给其他资源




SQS支持sample
visibility timeout，获取消息就lock，如果timeout就释放
设置处理多少次失败就自动放到dead letter queue(re-drive？)队列中SQS可以在界面查看
SNS no recall，中国SMS不能用，需要用合作伙伴的
SNS设置topic，然后加多个subscriberSimple Workflow，内嵌retries、timeout、logging
描述文件是json？

Simple Email Service，可以outbond发邮件，现在支持inbond收邮件
提供sandbox用于测试
不能用于违法行为，协议写好？user agreement protocol（我们做得怎么样？）
有API支持查询，可以查询到达，可以查询用户complaints

Amazon CloudSearch，可以上传数据，非原子
（和BI服务类似）


Nova创建快照，是Image，不是snapshot



EMR，Redshift，

Kinesis steam保存7天或24小时，根据partition key来分片
Kinesis firehose，直接连redshift而不需要用ec2汇集

Amazon Elastic Transcoder，数据从s3转到s3


spot instance支持fleet支持多个实例，支持block一个时间段内不会
我们自己填的bid，会给一个history price


S3，正常，S3-IA不常用的，RRS少备份


Trusted advisor


Fault tolerance是没有down time，HA有down time


高可用：s3、sqs、rds、dynamodb、elb、route53

RTO recovery time object，time
RPO recovery point objective，data


跨region，snapshot copy，快照存在s3上，rds也可以快照，拷贝到其他region

AMI镜像存在用户用，可以用在多个region中

WAF，Web application firewall




培训：
每个人新的实验环境
有代码、命令文档



