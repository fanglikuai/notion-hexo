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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLVVAZ7P%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIHONxqwDGG3t39g7rUUdilER1F3QnKB8x%2FC0dzhJW0OgAiEAsc88BM0Iqa9H0uvWmGihcu0bxCq5f72V7hKegPD%2BreMq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDPJDxbQkdqMasvSUjCrcAxsKyynOp2oGGMtigU78bd5hbXrZzE%2FK8MsOuFTH3Q1077ZM3emzi7pjAv5QgQS5mdVGfkQzciX3c2tARI4P4aTYUlzyRt%2FTbN9KTzGES0BuHLWFFi7v1fkz2sV%2BsZ8MDvdErFHH3i4YCDs3TlNK%2FIkT0eM%2Fw2fdeJISkyR0N4EzN%2FlYKbd%2BxfIgJ2xJZyZPERwrLJssN%2BtfLEwu%2BqJgkjVcnJ8VGA%2Bq1C1vqiIZNNiS3PP8Glhl7ZhtENWSUj5Dd3WkX5UOyy1S2JAr8dRJSAVEPRmQ6fOUi2cIzuPpqUUpb0XAQzYIK%2FT%2Fu0%2FVRXWNS%2BnTVo9xS1UBr8JLkFdoO0KmPRVq35y6f%2FwwL4mcDjnhAbXX7tYy%2FMAe7qTw6lR%2F8eIiPfrz46iPql8DeV73zEGVkD%2FaqcbiYDMGqmuyqej%2FAe9Sv5Xe0Z3AdT558733rQrOUN3quk%2Ffi5B6WctC8NYGLdGACwv0H49doeVrtW02160JeHds4gMQVqR5jiVaNVx%2BZH8c1syslJQwTr%2Fe4Qduj0n5g1MtwbYr7FDE8HO5Pe5OxZGFB6CVt19Bt5aB7MGeIjny0CeNwywKcQEPjAfSGA0FqdXyow34XAGLVKMZ%2BPo%2BkwJkfO33WN9UMLONzcgGOqUB6GPyPT0CZkHr2sFeYpJQPApdY0IszymMeB2g8%2BPYxUK%2F0glBO2njmm6bMdZhpbppJ4boMoTEa3m6Jm0fO%2B0yBRFT%2F%2FCxx5TU2qnCHOqkJlYCg8Zeg%2BovCHUrRJIG%2Bus2KUsG26hOovFplnLHErw36FQD%2BgqaSfnEeDG8w8iYAQTJNVGEzMF1zWVW8KUOwMEycq0yG2pWtTCvo5e5v87riAfxeGIr&X-Amz-Signature=bcb4db31b98b3b9152b8d203b5e1375ed31032832d5aa54451bd82c873fc6a3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

