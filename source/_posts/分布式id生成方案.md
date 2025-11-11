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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46652XGF66L%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIQDnOBBRp2lX2Pcy118ZJsMJtHXrojDbQca19dxJ9%2BEmzQIga0UQtzFVzeMw2IAXkZHwIUgrCx1M6djsy77R69pEE1kq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDAQg%2F%2Be6z6gH3kF3HyrcAzgw88GkGS9RmlPRMaWg6MRZ%2FLAEJyQQoLkJwwnDJaos%2FjC%2BUieVFtuPGKb2Wug7TYsiVlcq581Eb6OyGhQnOxR1PZqdOd%2FkLGqwgz13m442Z%2BIHWaYJ6u6B59RKQuZoNTX3NYHOvZcbhcSCegaWoe1U05L9O8FBRISuw1O3gfcZinIFaydJ544%2BUmUXXZ1Czp1WYhKBx1SNfmvnkUWDdisc%2B6ujrXPSMGLDJwkwB75JC6mhyvkqf7Y66FVKTePQ%2FTa7Ed3WAWq6zFOsH47ILoLewhBAWwviXrBaH3%2BY%2BFSFJzGAj0Jmm1NTFNV5s7nObq3aPtpxJtXcGQuWM1e3aJGNccpFB%2FSDUh%2FR%2FPeEaaMhWI%2BkVhU1schZkzib5OZAwhpMpGzD9p3TuoXp%2Fe2Io5kTcSvVRR9U7K2nn9%2BCkLEEBgWSdvX09WzqqM%2FvJa%2F%2FF3e3kl%2Bv%2Fhaeuy3wXa%2BJ9kfGOKiNOYNfesHl0%2B%2F3oN%2FONYA3aQSxW2RIYTZLnkf5V3k8Z6hwlx4XJb%2FtC1Yd4w9JVo7mMqxt2vGep20SrdjhEECH7rd9uGemURggikA%2FL28TXv5zdOJMwFTyP4cg9bWzDecIi7NnB5XXswp1KuZRsSGTOqosuf2qhjOeMPLnycgGOqUBzqj2o%2FDqzZEBUH6HIZDwLE7YZti6huYLzxVRapXGfKYE01wtfOgSTguPqklVsAhsv12vPJblVE5sm%2BRza8fFM29FSHQzEgCNwEQnmpSFiqCoXVEZmkpnkQ7wSMxO9vUWB9L4h4peaNXX2VH8mT5eZffFhHn2irwBiJiw9nen%2FnhlZdGSvBIV16MWFept0u1UdWoT3VFci0eS7iekd6Mp9XU40z3C&X-Amz-Signature=86741bcc6c63e0066e9d4a62af655e43226f8a1ca99a9c28b40490f78cb1106e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

