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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654FFNMPZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcowijKpCxXGn1inqZgfGJ%2BGKjMPk%2F2TuJADjB1anX7wIgdLul6ZpcoMtxH1f6vy3X3JpoheIVbDHWZqjrwl7PnEMqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBN7sWTscKE70L5A%2ByrcA46A3MAIuon8PIuXDOdzOl8gQ6KuxD8i5c0UeKCxP%2B2qWsvIleuBzO%2BwoMhNJOKr0u3p%2B7FRpoF40MuQf11FRjR1Lib2mtTQ2NJ9q6ubyNtxwW0ZeB6T9zY8BUW9cBLwMZCxFDL1h2N5ayLbi57ITnBbwDHplvxyP6muNURTilZLmGDemSgsdk0lqxR9jXZGsfZhRmHMeJJ25Y5vVH8kab2nFI39AoVlPb37S3it2VBwpkuYcHmen1s26TovrVAxsodhgyRZt941zwvPCeBwq%2BT5VWTEZJKg%2BgCocvm3jiNw%2Fvqp29eJ7oKNvI%2B4%2Bgb7zE1ARXDHs9AOF8JkjujDB6DKEwWGGZGqqDFcxTQ%2BpG4b47jjT0p63yGhWOInCX6Ki7ooSzQt0zG7T%2BUMrIKgHF7KEirIR59uAHyT2201ZWJT46XDHMquHgREj4I44%2FhaWfafi6FOdAZCYNM%2BaNomahWvmHWnd2iAjjkNZPnZdHjyEhfnMAX0HAweXrnCeGRnjU%2B%2F5i5wF73pdjiwTU17%2BUdB1aMUVtObGkYHY%2BkwOeed3YBM2HieE1H0El4%2FcWx3fvqgIW7MziGoRAIWfIAkoZ6OFQg9h86BesRBJU9pJcZuip50Fhx%2BVIQ4JHheMKKg4sgGOqUBierQjN01km92dsFSFwWOX49d0Yzb0aPYuiyRRdt9u%2FlNr8zoEGJTlH4ljC3UG6Xj9B1g4WSWGUDRCa2ODXsMmaQfXdOddsx5CN2uYptgI%2BixsJ1WJb6XDtH%2F5UlsXks0hkkTvIn6S9TtfjBkjoPAqj4DwkCk7nt3qdPzPsQXaEKm06%2Ff28q6qSs2%2BYGzyph9V9rWOgMv2vaTVFHjE6ij4kAikpJN&X-Amz-Signature=f174a8164652569b462aa011e6f952768e75c5505a8afbd6e34905f9f2647180&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

