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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZV3OIQ7%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCH4Jm%2BnO8GG%2FBdYGcOqP157qxASYIPnS%2FioeLlMVdlAIgD%2BO5G8kq3ALZFyFWcqyzEfKVbs1YXC6NFYCI4qc1Kcgq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDHi3jmaZkzjt6C7xRSrcA13aeMft2vE91%2FyMtqd%2B6GHpLu06OKxUroVk%2FZD63UQJqCw%2FOpXPT6oPHSqh4nhqRg1DXDyg%2BaNlkhLs60%2BNgY2IVJRXIrDthdyAGCmS7oaJLMQXvxEl7cmev0aQCdvFPXlQb0lFn9pYrSDJnbbgeRwFnZy0e0RrS8n8sihX8dwGobgki7HZ%2B7h8wxB4itQL5HPU75KxX2guIId4HoBLn4BsmIdpnnFM%2BII5wkPlklhQSZsKXY6xWnjUoRYJsf4xDahGXGIOkdSfxcgU21LBxrQOUm1OLBQIt%2B2l3QhlO%2FXtiSLJK8w1rN00LUxPFjnFMLcr6ZJbLgpFISm8J0CtezKZuBTN2aS4FDdFzKEgdcIT8KAUkdnp1asH4MFDGoSnHqCBjErdRAO6iCap4YCuHSL%2F7rX4RNtdRSHXFF6eqvPdQgdpibBXwj10nPQ2NocoBvmBDbCmy0hVDd7E7CMA8l23v9KKrM%2Feutwypta8RveDrsh0Ii5pJJ3pRazjWDJvQno29CY%2BLwaSO2DTde03Xe8%2BzUTT%2BXIbJrsmoVULrEaxlFSGi4A9K5wfC%2BdnjJ5vOATgR%2B3VIluF3XnvHPw6fAkh76106E6Tt5eE1YRQN8ynRqAY7rrwcSUT3VD3MI26lckGOqUBeIvGnhrI3MkocKoKQMEHR4Izd7xi56cjKbIG3pB8QQg0hmMihBk39qzH6NVNBD5IzfZXrV6P7fy9nzeS8zj%2FUR1fWaw9fzscnJ33lflf5PhWZ6HQ8YHbvUC%2B3xIKCtdV1N4IbAbqkvTSDaobJ7tQTXnlfkFNmUKNerUE7cMZP6Y3jpXVAVwKh3F%2FjeJXTYNEB%2FYsLadA9NU2sdkmGm53ZJqi%2F0dr&X-Amz-Signature=d9f86ef73d5d7bd21e702cf45eacad5028c4d852c4157c4eb7bd412ff4396bf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

