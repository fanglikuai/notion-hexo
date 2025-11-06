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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TPMUGB5Q%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICtdJfzrT2y061n2jJ%2F1Gop%2FPzNdejZxn0BiezFipig5AiBSWdgqFjJmvV5RSxxyejZxVhvcLai1Y1yWsYU%2F2cbpEyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhNd6B8uVcgECethLKtwDxvLjbRNR6eYgrFl6ktiMqZpR3GzY1KrgWnblEBcqewrC4ujWeNadx53ppwoOgqa0CNWPaxd6PFQUYP0C4QjJaLNzxrrqHUMarF0eeQrstDMqtZWDKUMJrSRxHLlcUgTCfIpLkqjQ9USHN9IgqioRkr5OTbnBO3io8JX4Pt8WUOnuVbzgeWSfSqnZS4rW18mu%2FgddLM4IH3rLJ6WpqeyYNTq0qnforfojHDtcQ%2FDlhzoQ%2FOYo77UQrbEyvQ6Uszkq%2BJMKYhL66qWHdO7mr%2F24Pn9%2Bbr7d6nQvQCeWQLKguqq9MT48AMm4DWUfjVj9TWvf0ohEbD57OH38qqrAx3KGhOjQ15JZR8kl0JfM%2Fp4gN5zPqbPqhDXr6yx1UiVfnwXaiOtmf%2FYR6XgMNdFhvpBXj2PsSSzAx4WoiLGiK24pXZdIlzl3%2Fi1zeInXsOXBjV1Q59oWY2N8o%2FhKqYAdHC8p%2BWy9E3WYDHMOWPtZsSDe4gMxgELs5Wv7Oha2Xx9YXmwkA7dBoLn0wRD7vf1Byx8KoeYDaykrnHsbDGY%2FTI79X2uedOItryyUNTYZ7h7FrhVzPFP3uIvlIJEHw1tVhgrl8UXmbISMxtN7EFES3AixK7i3h2Qm4rnq%2FfrFdjEw%2BqOyyAY6pgEdHKPk80cq%2BdGkTXJcW5u46SA0gRv4lAJcY8VLD8ECh7ylWfZgw1Ip22Zrh3jkMciOMBY44CwTn4kbkbKjZTICr6tZW04j%2FKtP2Y1VLz6nnp4TJOJ2FAe4EB89yAuN4vkPBpqcTX77jKEYT5sAj9Ia8SLnviw%2BmnTkOdLBo%2BcBCSclbTFT6YaY1fpjd3g2SQQdcIIPx8Zt3%2FDYRSkcrJdKL6VWro6q&X-Amz-Signature=677387f434194a2396bb18101c295c78d7da475b821414c1e8e2d8d341a4104b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

