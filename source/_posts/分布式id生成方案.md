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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BPU7FXB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T030234Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNiWTuOdNc3kmHRsvBcvTO8XBS6GAEsPRK12ykvEMbSQIgNJyIcrPiPt4C7%2BEIIDxIw3GqUK5GvdCnULdSPVTN00EqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ800jYwqoNzm3g7RircA7%2Fp0V0zAf4tZX8tpDzD29F2NRzPdY4GRS0icig2OJIKVZGR9bj3YANtf2mkAecAN2dpbgPqki2jOKgZgOHldzmF2zY7izPHM%2FK49ZttqaY%2FmMKPyLomRN8NI%2B9%2BbARWXXPUHVE1DtGXeOt691K45nsREUoeFFQ7GCyuR%2BN6yV1lM%2FZI0mcCmQRObjwXjudC46iwpMdvjDRHpQjEhMnZL3YyMr7h5gKB3Qf%2FldC1nmGwxvTYy%2F7K%2Fe72K0Ni51Rs%2BYHahO8tW%2B%2BcJRWTSOhsxM4FGK2AojzcQaghaOwixt0AreSbWm8zQV%2BVkeGZi2bckzpUT3Gm9NhI9dCz%2BoJPzkdEVdQKzdDv8Qxy5UAdtjRUZlmw0utJudGyexs%2FbLnW3pQfKzpLJ2ljl6zkkwPsJpf111eHOYAUpNZZih%2B0B7jBh5UlQneJCWZigtFdEX7wmjZWzhJpsgiV6y15gVp5QDYH8B8cDQfNaCGzbXF7wkVWLuqPuvEblo0g6%2BIceyvBmZKrQvYlOqyJrH12w0ligWjkNh0DdFMUG9VSs%2BCGZxtNOQBmFJ0TnGS8TWCapdit4b0GG9vlq0Pwx5lK%2BeN2cM2U%2FSbo0E1w46GKEw0cBAEKv2L1z4vRdbQtPrOXMLy%2BtcgGOqUBsuR2LKLvWB3Bxi9cIxQzOOHrpcceDBp3PoONK4zI6bzeVhFxFtOZbXUsitKsTDamq7gHoqM2W5LHrNQfrAwk8RIRxMpeAxVkb40Fn0M2xZFbeEC74HYQ7yWRRcU2LqJz2H3xc3PCV8PJ8ewJVS4mZ5sPVXlSlrWQeuEGxZ%2FXAr3JJtaCpPELN%2BTKDbRtTB5GtLbRQL3qj9dhJDfY2Zf5jC1p4IF8&X-Amz-Signature=134c597ffbc1fabdeb256232ba5dc6930080bf75451f40f43a65e01a1eea8a67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

