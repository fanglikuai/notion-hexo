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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634JD4NXN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T170059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJHMEUCICiwbfm2eZmdu4xi3D7WWCnaXEhzIUtwotjXJysKVubAAiEAna1nxb5%2FVE93fRmHVk3Ap9rVQd5xECeJJyHgp9cmoKsq%2FwMIKRAAGgw2Mzc0MjMxODM4MDUiDIa4GuT5T%2FUtCoBQxCrcA1aU24UF0vWab81uO8fnMwpJWs7CTthrW0vXOmSq112Q7Fx5g47qfSILiJqYbyB9NubB682U7XEjZ74DkWTcvNfPYpfhzChmZW0eKeNN%2FCZ3ONgnlA95nsY9NeH6xS20kcidxoBlnZ4z%2BMkYova%2FaFjqlimfSbQOPW1AyKDXlKHx5sfunrxIMXjnficGz57jorZlodS1f0CaOsUxXBX2pgHA1DyM6HS0c4dcjocvN0AKdDHLi1woERua4suDxMtCQxeEFwnnLj9b%2FDL15YgeXta3ww%2BXLwf%2FvlXcbtXgQkn1l8VOJfkR%2Fg0%2B388igrzTNSaGbBZ3JdvL5bmGXJY2bWUhEqoswMAkO45gOLCP3EUAEhu2PP0dWSZVfYcERyXfUUATzvSNoXSzjtAJgu7RMGCG5sB3dEj1wUJf9vSvrk%2Btl5u51%2BowVPoaymHH2oudoOtwmV9%2FOimxUa%2FJ0dOCrKrxoO0xYdE%2FQLFq%2F8JKBKBD0PAtCIk1FGPV%2BUd2muO2QCtie5x4XFGK3Vp0tAZjszafssaTM9ATFWYYLOMYQk0X67%2BUFR9LRItB1JwK6M6HIXOBDF%2BJZDvacmXOT6t%2F5%2BzlSWRNjpmRx3iqUTBXWXuF4CLlUPWoNDxXENMeMLvDh8kGOqUBhibzYEnIjCypvaUYcbPxQAvu6HDhgJZhRfgRTAs1Av%2Fyt2bX5UzqfoYVFz9btWh0irTRkj0v6tHI1zG5C8wP4EZE7eKTc55056RNAq45bMNJ5RkGRFCjeBIF%2BQgl262FWSBjJc%2Fkn4la8ThZecI7i1hsit9FR3Cx0wsDCWAHWEhc4MC%2BMCOjyZz9lWS%2B9gvrsOC4kbl%2FZJdOydHgBPx8RSuq8MYy&X-Amz-Signature=95b2fdde7060fcebb2a6ef60ced6b3416de3456ef1ae01d9e955b078476d5b3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

