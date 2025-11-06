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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5HT43MK%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbN%2BM9T30f4dq26Z0w04RGUYapALhpuw4DXBIbs1l2WwIgNN9Qq%2FJBGeL%2BYouGt0S3CMomrACsr7yh5qKUJHagLT0qiAQIof%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB%2FH9ZDzndukDCfosyrcA2aVfoZsLzagkr2PTQpRfwP4aTutJXSPQ%2FYq0rGaFRHGvBaP2B6RhZzakM9jRRanEu%2FoLJGe0QKlx4uMcNktmu0xEsocIqIVKlr65ZMEzHDDj3qywJiJfYh9TFTTUWYvzJFw4XMO2uDPLwKxRUhF%2FSflfiCkXkcwjS1AkD4GFTeIRwoSQml1MuqmXEl%2FOGx6iOot8vcvcumo8r5UHFpOacPo9%2BHMmTbFdCy720pHkgp7EPEh29hPJDjqSpuHJLIjomcBAOYISuODobv1l3BDAM1OLtqyXADSQ4%2F%2B7yfWjvXpExRU%2BABavcG23qPHPJc3mYHX%2FrSw8XWhOWJZOShMx5kUxdUquNLOYMvVayzlbj7bQixLyK9IpZQh4fwq%2F0PA2gcOJUgM2KfII2Fzwe9NL27NtljLZi0YMCCx8a5K1sKWn%2BbfcvLF%2F0B9UE03aVFhUdtQjRJSsUSw6xXwL6klBVOogtnKNvJow9ct%2FGL09HhlcMhwIlS4NT9yGLWZFhG9cled2y5NUGMhisZ4UP9YqLvabaZFxFkjk3Y7%2BIO1I2zdbo3bwfIswT%2BIcpbYmKRhbwgq%2FKFx8pFgNkwj4%2F9eeYu6b8oxFa5jYrl4sacrXaMKKtprnRt46NAlNo%2F1MJ2gscgGOqUB93xLuemZOypfxfGJT93eG0rwqVS1aAedTgAp6c%2FZHFkTKXXogv%2F2y%2B0POTaVegm%2F373XDFMdcXWXsXiK2LeJ2Qhp9PDAUzYDsz8iNHl3Sn%2B0DEgHfGJQSGz3jVDsFx0lSz6NrLDo06e5qHI1VEhu%2BqVgm2oUS1GnVLkALzecJRLGd769dfrz9PQJmR5OAxeYYQ7tVgXS3pEh5VrxBbriNtdAhVtp&X-Amz-Signature=bac2307acff1cb3763e6ae5cb6d5856cce94b5e84c09a12e1af843543cad0552&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

