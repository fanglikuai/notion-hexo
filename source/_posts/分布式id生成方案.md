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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SITPB572%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T090056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDYYQ2r1Affc2N38Bm7xfXup2idm8HFjx6TNMi2SGhV5wIgWm7FtwupgUSs9Hb3Y%2BACzQrPAiUPyRDMFBBIsUsUd7sq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDG6sgehEWA%2FwE6bpICrcA9PK7%2BIfQ8wuB3RRMBss1kdFFkNc3pR18tzTwB12nf7nLvuDiLfgGgbeMB9ykF%2BwPMPC36hEnoAxiaMqKddHPZUy4l3JE8A57FvV9jdcEqYoLPE4AWXPSHR79088RqWRMUR3D5hvU27Eh%2FKOA5GXo1KDOs5iUTODjA%2B7CcuoxjwQMBx6lTWcXCW%2F0%2BrPoPvjq3lQRIXLeyQI%2BVq6SIijB7E3eSt5b66VCoRVyEqacqNrSdUIrXMBPtwZoykp9FNLrnpdShDh8DhIvOphXHpSg4eRW%2BOzeXWfhqWYu6QkummZnLvDprYhlHz%2B%2BRGu2hxNkg10J3q27gHTF%2F62b1ffMnS22HUwhR9wyno6s3otjaERlTggaudNmid6Qt7eL53DxCJEK4LjQc9NzSHy7T202h32I2pR6R6ZmJPLOMMSc%2Fsn5VfI%2BDyQTZoBGKgho9O3L9of9F4lcgGyQyzIsIrPnz428HbWrdVyKkU%2FHXkBnyGQiPW%2FPLIcfDmtZgfdP5c4QY05AxHBbPmOUGMOATt%2FKRFeejifiYilHFydOzpZ057TAzHDLQ1RDiMvm6gELLZOZK%2B2RhhQ3vlUE0WOO6zoOryLpkhod6tis3o5JNUj0NVyALHXOBZUmZ7%2B%2B2fmMNeXi8kGOqUBV6wJSTNu1jf9MgD%2Fu6zH5q8rFmSyt3Dnz%2Fp1h6XiyHgDrZFLQ4KvvjlqaWl1Uo0CO9i1k%2BojqpTbH0wChyssPxGqQ1JsMSbNF%2FDfSTQ6mu5JI2BYEccDagoH%2Fpkjx3fWmA69FaxnyHef%2B%2Bl4NF9VAYy6D4Rkhb9FQW4sXQd5l5MusrST%2FFv48puU%2FWMhJnOYAWT9cDbDxv5Gq9IhJlaE39ocgdNi&X-Amz-Signature=f7743c18b6ac925ffd96f6330b600bfb8f51f7a4c78451662c438dd27799a8f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

