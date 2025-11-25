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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QTX5BUL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBMquF2L3bZMOpjKvtndK3O2U5o3foR%2F2VzYHL4VOBHwIhAPt8AIKaKiD8t9oyrP8LXbz6TNt0F%2FM2y%2BKzFZCVOgFCKv8DCGwQABoMNjM3NDIzMTgzODA1IgzMowP2BJkG59oJsKQq3APtUQyE%2BcK6CoFmeRjrJImP12UavmDA4SEsAX5TXIb%2FMulsoqNuql9JTA1W%2BFWKQcRaI7POSBWyt4PXFb8FefPyKLDMuQQKd%2Fe7ce6Ak6nvcgQ8SA7OVDaWGOFgQgv53pkKPROl8H0sO8qh5w4dOelJNX0lgN%2BHRqPm5sKJJsVaYUr0QMmaA02ElKg%2Fv6fVhOmTpUIrNfysPP7CkYHYGsPY5pawQbZCLlZp4ku1issIderLEOk3NmYs1%2BSqE0lhz5YT5CtxcqLphP%2BLyNiFrCHp4%2BbpP3QNqPWHGkBFRf1NbeG6CK63YtmKASV9BvtyOPcgvdqyui4jr5hW%2FzqRQBQXZW1mtC%2Bmryzk4sT1a3cNtmiSr6yAF7%2B9Mq2nPiCjF9LbrX7WgSiXraN7EtMYXqyj3K64hyBxNbWan%2F7icLKxK4%2B87%2BVHki9XDDLGYhr%2B6ZPfUdDp6FSvCBhQFxjyZy97IfXkMH6wDF9Xc0zA9PjBnv3ErFTHgGz%2Bawze7I4B1Vord8aIEpPnX3ZXDZOrdRGNoRKL6L8KuZFThy%2FI9yf2eHiQEvQN9Xqu8tFM6Ko%2FsJb97Gh9eIxgoS4JyF1u8k3nlLnj%2FpZhg8toIFrEoKe2v5hrMJJ6Ar4ZJVaQKjC1o5bJBjqkAckLsC2OCbqZnez0K1ozgaUVH3Mjjtx1nQD1BnYGHF6IB4zfYh9RtCIcR4y4ZAYPlPzBnso4i3025uRx9X7BAwgBJWhJmI9MGm1EgCC6tHTqke58ilMGqXwx7pG5UkCvnCmfpfShI18IvNiOH2vabrxSRWyJ6npsgOZ%2FNV3iK33aXg2uT%2F1TXQ7bUG%2ByvS%2FhEwIQxRY3yBJb%2FkjdGN%2FnjuRAmJom&X-Amz-Signature=dc4aa71d3129b17c648743cf5cb7caf6a9efb69458ec91479c713990ccfb24fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

