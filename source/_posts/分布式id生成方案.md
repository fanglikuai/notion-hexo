---
categories: 整理输出
tags:
  - 分布式
  - id
sticky: ''
description: ''
permalink: ''
title: 分布式id生成方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QR3XN6A%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDnVRpxS7FM2jSITo2nJ02JIfF6zwEL%2FLuXuz%2F%2FU5cnGwIgDPNgpYmG0Ne9%2FauAvV3vr6jjFsCMiTCASuEtBvq1zh4q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDAvTgw6lbyk7NDGJZircA4hwhd%2FLoC8bNR%2BVRDecskI2pCAIjpUHT03rIgkM6Nk3lb66g1NWYfX0pU2TV%2Bqi5VmOiP5hXkKE968GpKGDQ5l2ZSJmct798%2BsaJgCYGYCXBZh0NzkQqDMGB3wMRPpqL9kOlHkD0lPXXLYYFKc%2Bwa8Cl0QPF9fz5nuOf%2FzIiCPnUlj9FftueaZVCqWn04QuV%2B01s%2BB1xqTLuVKOpF2xq32Bg6970SpzU0qU3rbiy8nF7rHcudThq%2FEAh2%2FfoYTLxbvmtfqDou2c8RJhRP3dCWR5nwMxWUMYLVte3UTBRxC2njWMwGblhqmH9U5eixF%2BT0POM8pubfnU9uFDeDjcNuVAYYTcx67ToA3xijAn9nVnYLFe25vra8V3wzu9maezcCWsam8tLHm%2B6%2FeMUC%2FBahi%2FIDywwqQrMMeHm8GPaywxzrY5IbmtlH2e0S9bQPvn5CglMzNeNBDheS3od5FsvwGrwak18gc9pMcH%2BuYTS2jmxHLsIcHh68MMKQAxdHmd68aA%2FgdmPzwj8Zsz0jo%2B3%2BN91YDu%2BAOSKqpRDW2DGYfXU0kZylsnil%2BpM1sy1UD5Pjo%2BrS6UY8RyqPbdxqI%2BKf7RsO%2BhLKoWSsA4n52CbFgiB%2Bou7B%2BFmliomk%2FDML3Um8gGOqUBeTOfM6xCGfY5DhW%2BEQgLlgvFyX82e1qVT9hCZ5x3oMYbr8k3R2N8Y%2BOW9xnBfkFcnA4nbrb7fwKV0g7cGGaYugGNlFQiJjPAvzJNAxCPi1%2BZgfUsR%2BE%2Fvnac4TLRP0RLb6VQa8WDu9WsFWKigScvwHutY7pQHfpp9XIxM1vC2Tp758xLTr2DScs9RwD1qe%2B%2B2UeP0FgLZG1INs06XqGlMeUVqnMS&X-Amz-Signature=dce2178f3230fc11c87851510c45e28b4023967766cafe558c87a33e5558d575&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:55:00'
index_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
banner_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
---

要求：

1. 全局唯一性
2. 趋势递增
3. 单调递增
4. 信息安全

# 雪花算法

> 0 - 0000000000 0000000000 0000000000 0000000000 0 - 0000000000 - 000000000000
>
> 符号位 时间戳 机器码 序列号
>
>

12位存储序列号，当同一毫秒有多个请求访问到了同一台机器后，此时序列号就派上了用场，为这些请求进行第三次创建，最多每毫秒每台机器产生2的12次方也就是4096个id，满足了大部分场景的需求。


## seata版本


将时间戳和序列号连在一起，然后每个微服务开始只获取一次时间，后面直接递增+1就行了


并发量大大提升。


问题：


如果超前消费很多，然后系统又重启了，那么是不是就重复了。


不会出现这个问题：因为这样的并发量得很大才行


![imagesb91ceb2795145be127635d50c861bbfa.png](/images/c686239b1567bd4f1ee7f6da809063a0.png)


最终收敛，防止页分裂


# leaf方案


## 数据库方案


去数据库修改字段，每次获取setp缓存在服务中，减少并发量


重要字段说明：biz_tag用来区分业务，max_id表示该biz_tag目前所被分配的ID号段的最大值，step表示每次分配的号段长度。原来获取ID每次都需要写数据库，现在只需要把step设置得足够大，比如1000。那么只有当1000个号被消耗完了之后才会去重新读写一次数据库。读写数据库的频率从1减小到了1/step，大致架构如下图所示：


![imagesa73fdbbf512d967222aafea71d8485df.png](/images/6ba8f71fc9902de8f6ead17de802a727.png)


### 优化


就是监控使用的id量，到达阈值的时候，发起线程去更新


采用双buffer的方式，Leaf服务内部有两个号段缓存区segment。当前号段已下发10%时，如果下一个号段未更新，则另启一个更新线程去更新下一个号段。当前号段全部下发完后，如果下个号段准备好了则切换到下个号段为当前segment接着下发，循环往复。

- 每个biz-tag都有消费速度监控，通常推荐segment长度设置为服务高峰期发号QPS的600倍（10分钟），这样即使DB宕机，Leaf仍能持续发号10-20分钟不受影响。
- 每次请求来临时都会判断下个号段的状态，从而更新此号段，所以偶尔的网络抖动不会影响下个号段的更新。

## 雪花方案


使用zookeeper，在第一次的时候进行注册，然后后续缓存起来机器id。


后面进行周期性检查：看时钟是否回拨

