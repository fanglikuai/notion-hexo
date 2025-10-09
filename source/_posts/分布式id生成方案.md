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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLCDBYK6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCICIID%2BVv74%2FArAptGoDjzupJuA%2B5k5n2PDDn5F1X4Va0AiBMDtZDDyhYXPNd66%2FyowDsWrGcpkA8p%2FMPHmMH7GhVJCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHtaGREQuekLPM6Q%2BKtwDrQD%2FU5q4sQghkeqEv%2FuN7HjBZzzEw5E%2FtYCJTkcWShOnzVdeZL4D6kiugBmWhfmOuwVYwDNktIiQ5Wo%2F6tjUBX3BzrGrVVJHcR%2F0255DTv3i1jt00IYuNtFrZwDHIltygDVMynJ0g5ZkEgv1m5qzHv59HEMGvIvQ%2Fz8Wt4tD%2BAGEeA%2BiedZtYfy%2BsSK%2BJm6V9CgheTUOoGkoKzc7TD0t5Uv9OE%2F7Etqi98xNl2FtmlMgBZs9FqmfKw%2BpB2E7NSOza1wxHG82owz5ifeunQ%2BOsY6HWPVT1NuAFFmj5TTac17UyvOemJMHIyUnYGKVPlUnXOVZ4iMIfkGZuVwOFIPsBpHT6J52MkhogLB2ykK%2FjuA4wn3hzsX%2B%2BGAqnkvpOiLbrL0H%2FXoE2mnPGmj1OSKwXAM7Faod4y%2Bf8M%2BO%2BPPJHU0z%2FgQoQ3RrjL9x5URnblTcuWmhKGyvDyU4X5zUgMcWjZDsCCMl3l4NblAi9AeEWlhhDaNbmss%2BcMtB%2FouDe4lLF0ufR7YCEnQZeUjsypu%2Bsn4fhevME84sIjXGupLYd36c1FMnP%2FeFlob%2FCQMJcnIDmZ7sztD6gIn3Bj3iE7UwIo6MA%2FTa%2Fn8T8yYiKBD0x9wypE0P%2FGqVxulleW8w8cKdxwY6pgE%2FxE%2B5hBY607kZ1Xr2ogJogLFdTA8Al5BZNehE6D5lVgl0H2mPvE6kFLN4WWRgJFU8MfcGIDqpnrjx7P6vpCUkNo0jXmXpa18YYVcIqwXzSqQyPc2cV9K%2FaYUcC92dO64JR58HT3fV2zK7wcPiTK03MwnSEkMOsXH3C68iJWzJ%2F01V%2F0YrkTkgh9DSmzO56AE8ukRivCVjMbo%2BZ3237EQ%2Fi8aT29Vw&X-Amz-Signature=ce20316af3c8fc0f5389065649cb76b18a9c7e8c2d1e68c12fc965819cb8815b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

