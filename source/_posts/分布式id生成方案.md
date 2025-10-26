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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674ZZ6J2W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICjfWpfZ45bNaqu%2FMVQwT%2Bb3GH%2BsRzVsaEW94qRVtI5mAiAs3Gc7L5EwxEbR0%2F9N%2BWsf5GKPujHwyVJegIhiqcUo6SqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOsIG0A%2F%2FxO0l8L5zKtwDrZ0AwYLQViaNlIPs2%2Fs9KOwzn8y1f2Xm9cZ5JWuB7KKR1XowcE8iJVS0uBsDiYzzq%2Btx3bPq2dMKbXW%2FR3Ch4HtrGlhij7SKOp%2F4BzcYrmdgAYAxSummWmDT77tkGtx%2BpMnsMy6H9H3ATkjmtah3h%2FF61RhvGr%2FBQEX6YMDiKsmG9cZPz0o7NDZLYG7W2wq%2F%2B9Rgd5nxroDlofPVCxn7KWTi%2B3zNjoLJykEm2ElobIyCRtKo0Ezt2gpTPXPR8L1C8R50%2BOKpK0hX0pSuE4ZOORzjugNniBgIFpKlikZaKVRRhCxRe65CG4buwz7w%2BoEpIdS9Z%2FO0IXPbZ1HHInphdrZeNta%2F0MIA7ZY45rC11fn5%2B3YvqphY04Pg%2Fkygk5sCtpeqtat8KOb1uKWPJ2Oy5uDtNF4zlU3d%2B0V2jVHTGVBWJWvrVFLhMXGhhynxumTdS0%2BcsXLmB6ejcN%2B6WHegCA8Sl2%2BcCnyv0CI1FEHn8by2DXFkS8MSyl2PrULXUFDe%2B%2B4%2F6greg%2FzTjlY4tEs0g8Wzp1848KnmT91EdZ9XdjXMZ1%2Fo1Fypxr1U6eupX5vvJVzhRR4zudVr02BeSB4daeE9H5vD8hjPxAlVb1bbj7G4h9PyISZraB%2BiHUowhYD3xwY6pgHzLeuUvb1eate741xfd0KNMd7vvvvdDU%2FsGd%2BVjTijurh0jesG6Ba9rqbHT80F18PB0yg8a1RMGm4vgtzk0xte0ezMq9rT1LxplMf2pZ3wsMePb7Ga8%2B6QIh1TNa51nMz9oL3xWt8pUoXlR0kGuFovjUiaX3Sxf%2BGw7vvTNnEjsMKHqgRqC%2Fm9ZHuy3hShi%2B8pK8DlAUvNqQnZNQsILb9spFL%2F9ozY&X-Amz-Signature=23fa5429359433566bd0a890d2064483ad7331adbf1367b87e860e5892a8e8ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

