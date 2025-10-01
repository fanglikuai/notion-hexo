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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R3AXHV6B%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIQC6%2BmatTIsI9dGHgpDinQyHL5RZOGRXWTlXQkWV0%2FiYagIgVWhb4PvPmHbKjtvi4u1G5c6QhKjC%2BiZCiAO1OCTvNLsq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE%2FtMSvgBlFT9m4nISrcAzWK9H7LL6dejWLXed1qyecp7RTtS1dzYq4KxixTsd%2Fso39WglSP5Pd7nHiyO1eKt4qIjW%2F3rev%2BbvS5BMo0%2FgQN7kRtHUe9hahXTxlAWWuF%2BcVgl5CNnqFMZ%2FxituBs2ZCKLHtkLPzR1O1lA0ibT8riEyMEQ%2FAs5YjuZxS2YNN533WebcvRkqR3jTzrttz3Kt%2F9QTAikgH74ie2GvnqYyLW%2B4hDC7VBh5d9rDnj1aCFMvht9rfTCYeDj3Wkj6NOawG%2FxeLgT0rO2drh%2BXgLli90jkR8r%2BUL6e%2BQV%2FVHJrtz%2FWrF5X68YjodQB1HoEx%2FHWUxBVu0yllfSxQoT9CnWU9TgugYCfCtQPwjC6lX6lk04%2FmZXyVPK4qQP9cSEPotvn8HTzjhBtH13A%2B5AydDtHkcwyDEI1k1AbEGmsvwnpZQAReRVcXV1xLsStsFH4JITIrPFQh7dV%2FboppJVI2vQynYK3wbLXfQxZ1DqRgfphRWV0%2Byldb5aOSCgOz%2FKTCBmPbVWaBqn3yL3SndVeCPjhLYdYSygKUS7gKaIxR%2FZoE3FjJvx0sdOtZ9a5ESYxpuLQp4WNTH8ZstRC%2Fn7GktNzT4Un%2BkS7GPeIvMNsJMmMgpWe%2B37kS2%2F462dWn8MJq99MYGOqUBkw6dGgZxWsBqCNj2ECBjL%2B45WrvWSV5alZZ2lcppUaVXN5Stp18V4OLhpwmRHDsyOACC8hcCJyNGg0N0P9tAy8zHoVPCaoyC9PIP4CamF4pO%2FLCQD419i1ZZXqZNZkBFs%2BXI27oXgvb4aY%2BYPWSI0qGU9lFkrU1on%2BnCI14jwkIifKpXaiksL7O1AlwU%2FHWeY33Ij1%2FpW7mfFKKRw%2FIMjsLad8hz&X-Amz-Signature=e7cca722ed82f15ec4c26a65d108ab80283f8d30167e5322adacf27ed0ddf145&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

