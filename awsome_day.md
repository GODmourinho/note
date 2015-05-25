# AWSome Day

## Note

mobile app

availibity zone

maybe sdk for go

wechat plugin

storage for cold data

storage life-circle

auto scalling

PVM to run instances when it's at low price(Provide hook before terminal)

create vpc or vm wizard

direct connect to use single tunnel, how?

## Conclustion

周五和家英去参加亚马逊举办的AWSome Day，也就是一天的AWS服务培训，这里异步给大家分享下。

首先不得不承认AWS在这个邻域的领先，用户都要掏钱去上课学习他们服务的使用，还能考他们认证的证书，这次我们参加一天的培训也似模似样地发了个AWS培训证书。

这次就介绍了下AWS已有的服务，已经发布但还在Preview的也没介绍，其实都是大家很熟悉的S3、EC2、EBS之类的，这是广告场简单演示了用法和场景，为入华打好群众基础。首先这意味着我们在中国不可避免地与AWS直接竞争了，其次AWS常举行这样的活动有助于推广（免费培训租了个酒店还送了一本AWS培训书）。

我记一下AWS已经做了但OpenStack或我们还没有的，其实都值得一做，如果有时间的话。

* AWS提供了移动客户端，他们API做好的多支持移动客户端很容器，之前看到阿里云也有了
* Availibility Zone，涉及机房规划了，自从AWS出了以后这是不是IaaS的刚需了
* 冷数据存储服务Glacier，能大大降低存储成本，不理解他们的实现为什么冷数据读取时需要先“解冻”到S3
* 存储的life circle，用户可以自定义存储的对象的生命周期，例如存到s3的数据一周后自动迁移到Glacier
* Auto scaling，用户自动的一些列动态行为
* PVM可被抢夺的虚机，允许用户讲无关紧要的计算放在更便宜凌晨的虚机上跑，Google已经跟进了
* Direct Connect，用户IDC用光纤直连AWS机房，AWS只提供光纤接口
* 还有创建VPC的wizard，我们创建虚机后如果可以引导用户创建IP绑定IP体验会更好
* 还有AWS没做的微信服务，Ucloud好像做了，直接在微信关注就可以直接获取自己用户的集群状态，比客户度易推广

其实还有Lambda、ECS这些Preview的服务没有介绍，不过已经够OpenStack追好久了。很多开发者感叹云服务只有像AWS这么多基础服务才能组合玩出各种花样，我们加油，估计这些服务社区很快也能跟上:-)

