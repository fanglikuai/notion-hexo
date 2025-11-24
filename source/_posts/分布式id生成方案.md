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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTMVQHCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqLWT7hdRtmSckIphqcavrS7Pc8aoEylypduG2AW7MJgIgTSqYBVmNnk0emdd20yGN4T4X6NLEGAkcx4MuMBv1Zpwq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDLjhhSng0qfcib4tYyrcAwlqfSz4uvEHGEa0AwcCrJhNEBjuP%2FvAlFjZv5Zhm32k2mVtJmvcnvyRVJW8FbeQ%2BkwBNoi6dhtu6U%2FV010NReJ1cacpwEGIX9IJyuJCOfzReIYpg0wuAmZ5nqt65IItIih0%2FBGz8WP8wb0BY8sRfTFbO4Sh040N1w3X2P2%2F9n7%2F%2FyEwSbrLv6108CREaHMHrvoWQ6iU3LU7THkyjw%2B6xNBXlo0UZvMjQbk2czXTYVAI4Dd3gaO7NKAKeLxYl1uAxypDoShY1%2BuQgBMHcxAgKc0lAEKMU%2B6gGx%2BDONsgxt9jRw1LPdQLkk1hziZfIHGNvosCB0E7fjO7V0BZdc0NLxNC5VFNKW4NTf48X31BGPJpienNBF%2FXI%2FppFNiGAXyleofWMxcwjLFd2V0DV29qF68IGLGu30jZsQWYCZzWVcnMS8EMIzzoeFr7IgrBG%2BYqi9iWuSxLVuCJnggy7ashg4wSbStzwXghGcwCcUPTWibkEekcPR6LKuBlaVz7Gg7HL%2F9yiy4KKXiEA0d%2FM%2B9mNcaQAjGwSbYwvztnRp8JtWQxK6N8GNR6GYLAJpjucwHsEqALvdKMJTXcl659rRvwqdtfrK97494Ss4zo01DgkkRTPR4Y85VGtLwc8UVKMOXkj8kGOqUBl4BjCb8LKGp3%2Bp1v5PcREZ3%2B60gKtriR%2Bu4k5vzpQfjCt%2Bo4ioz1Fhiryo8VafMQ%2Fh9FA9XfadCQfq7wW09sd3pVsK68ICzS8vAH9xpwFT5f3xocQbRkh2IKfAS3Gw2sWMWGedq08myEG9Nh7SjjSDI3tebwm%2BHH9cBiOSvhHqwOqBEF84fp8hHRyxKu9EILubBCYHjRhh7UIOikPX8ZB4MRTbjr&X-Amz-Signature=1bdc66b72d74891ae6b64e14a9e2b9effedc969221f604fb87c380fedc3e734d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

