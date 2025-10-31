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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEZQPJAG%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQCs3kcZFPgjUKbdxyxQ6mNDyLgMJ%2BUUfR2rDzHAv3Fu%2BQIhAKWDOh%2FbabqXGFMUZSIpmPrAxCvH0TeJm53YaADkF4cMKv8DCBgQABoMNjM3NDIzMTgzODA1Igw%2BYsOVv%2FA861XBptIq3ANZA3USfFcbWH4dBm60GDT1BlGQqSWE8O22Nwmp7Sdwx5nePHJLcv5Za7DWmA4EC1qTfaTcM79h9nrqvHWc3jGb%2F01Sj00tzdfgJ4wuG9J7AZH7aaudJwpwRaaVkFnN8iHBsfxsCteqb%2F%2BmVWBTodMvjyo%2FVwssv9%2BKoC1pS6is%2B9qgsjumXH2kQFB3fEDkH9H8qqV3NeoXfHCwCiZti%2BlBPcEgDNEQE8P0yZUDfLMZSFqVNdUp%2FyKudgVgRaWaItkpXJ9FFUWxU1aUcrR%2BmOpdjuMpF6W9N0%2BOf%2FpIExq7l9b2nwltQYCvZhf7sIPEFd29LFMm%2FXfuHWkcp1cDE5ra0hvSi9FkFJ00l0jRl5HpJT20BFK9tEnhwwdysO%2FJ8xfYJlwfaWWYRuGyUPlchFiIxH7w8uV200ufno4e5lsH2BRong8A4h0LYVG4mzhRRyf4EWxzjIq3nyevhQ5RK8ET6jwXj1HsXcWw8i1H2iPVjuSRWyR1rhIzFLZhaj0gHjVXpUAM19t7e%2Fw19gn6Qma6BcWAsYRMO5yBRTLRlZt3h9DbNkFHMu3BaxbSR2foopfa0xJdAiR3SGLIu8VfpiFsJ%2FOaKQ3JX9RFkENsh51KvQ55a3yJd7vTiXgOYzCsmJPIBjqkATiX5fxtTiS8q3fs4meFRaW64nE0Fo9xabKY%2FK3ZbhFxX%2FLDb9xrwJx1yy3AMMtxGMfhn6bJQQ%2Fmos8YQbrYuNoFDGB%2BS998NxCeeQZZLy2UfINYtqWrD5sc7vH16odPgkWYovtTJAvQeQtlmI%2FjxUsoPDLhO3RpTlp5ksC%2Fxpbeh7mSrtRVZteZVVDyUJRX6naZJTchwUm0GWcuYSv%2BhXF2tSji&X-Amz-Signature=02b31127981e781a14d79c5333e27382330f5ebbd050e1c0988a442cdb2c2d2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

