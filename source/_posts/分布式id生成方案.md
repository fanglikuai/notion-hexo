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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZERJOFLO%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDzz5Vye8ywz49iOT0TMXb6N0kswqA3IdpvZzII3u6cnwIhALKaUYxrhe9sLhtLBtIkWY%2B%2BuHPn9VaYV7%2FOm8rXSumDKv8DCDsQABoMNjM3NDIzMTgzODA1IgwcPhiH3WUg84KxGbQq3AMjdwGCHOlWHMLuEayzXVp6bY%2BdRJaq1%2Bk7XYmXZoguCYMmMX4tneP6CI795NQnn2U6wZb7lxKbrX6MtbcYqyl8jBeR7FO9O6ch2sYiar6f9cWw1WhOMFlknZerEj4Dy9YDHZ48du4xt3ktzN1qqA9IxrfPQ1hZEPJDa1j8%2FtyLe9ZanOK4%2FSbWkFPGV7Sb%2B%2FpE9HPQB%2B%2BCNZ2TfEw2mdYBBV9ZECBWwx5971HJR8rqSDIemH5ufV5Rt6tiNByHrGSEqB8sv%2FwOd%2F0%2FhzMdkvHAY8vhBm08uoe3gyLsYyoUuc4OiGdxPiWAzw%2FFDaG4xgEjLPabEwlbUb58%2BmUTkjgzGfdj06LIkX38pVFuLHMCj6WMTDmnUzvbz6yJOFQ7q5uws%2FLlkZRvitkG8wE10DgYYUHmAwxX2azQOxsZvpdOmu4UddQ4fFqid99s%2FxP5KPDuDiz4Rotie5vy%2Bgye9H%2BU1%2B2N9Hk%2B7%2Fm4rIfB753X7dleCLSneIIj5dxroguVMJoEQH5aR4559CHEOu713xJtwEU4E6KNmuDlqvIU7Y4Z5zwZSAVfHiPi6bXQdje%2BogVtg5xtMTO4JPE8kHMqvAyQ5ltEUv5iy15lDUTOjyrPyE99JYd0PTQ98VchGDDG8JrIBjqkAQ5QiDS%2FOOliOn5pPh5H4bqmB6QATfNRfBI0iPSQXLBQfNdVizq3ilvZvZdBkBbzwGpbe4zJ931c1LtVltRVmihvxYMurYFLwzB5mQEEEr3XwWcBSpO%2BEWsNJ2O5saHDbTUGF%2BNmfQbuYWLkstllr8HX2wWjZFQ9THkiFfYyxtKLW9KkbVa2hE5HnVAq97PPbB8vTo5NJrIU8rKicjMpdZCruqsL&X-Amz-Signature=cff9890b04566699280dec114989b53cefdd9a228a99b2ff4034430d9c1fb84c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

