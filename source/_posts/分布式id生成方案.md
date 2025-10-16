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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCLQWTCJ%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhvY3N2%2FIOymW2S3n9YfGslnRWjq2evBmu7oVBkcFRGAiBTt5iQuT7FLYTnJ2hq2dzXhJjWsuiwSt1dM72i0BjhtiqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMftbvw96Efqz%2B03A%2BKtwDC6VrU%2F%2BDseMEYlhkulp6BQHt9ScOF7fU4Qogpb7P%2Bu8DbdmxL%2FxtNZXvPiiZ%2FvBfXdAEfTIBgajAxF7DN9Awf%2FVZ1wTdaWuwhy%2FMuxjwD79qGDiWHzATcGZ7AxUy%2FR86%2BvJjek%2Bta4mtDsmD5w5CX4msRnBDNUW0N1d9al4mFhyJVs7a1PTp82E2yPJtkRzE1HnkDcWDovNNihQFqr4bH9cgmOyE401DiVoYG1%2BBOu8c3rMzk56qNlbOyXJYz0L0hXW9S2n0NSjUOvlK0KSim7H3%2BH0KNG1nUW5E273JJ2txUTjpStmRFRpwOhw1CGUgI5hkcOfMmkZwUh4PTjWtVrwQnkcerbAMiZXCt8OJUb%2FcTgWhz1hMyWPvJZA0Lda97VlrYgFkOm4jYL2baW44mZvQiX6PzF%2FWVno8EFZY53ErTBWSFNKO%2BWLemle6%2FT6bWKenQ%2FwT%2FJExUqBT%2B7Qrc1lan0PG19D3DmgG88Gvje0tWZ9auQfyPnfpuyYQVmFtyNZsC6CZq05sOO2ZkMmPd8xXhpYAgh4dve2aymVhlAtiyoQwyrAijqUk%2BYF0US7UiB0h%2F4t902rUQuPASbYaTem2Rv7WgvnAT6zmNnXYx0RUnIlNhR1BPaDoxtgwnpjFxwY6pgEUeKiSRRND3%2Bitvgl%2B26PyrhyA42d0JlgHVkm5WOSuzwtc1anHqIq08FkETAwnBQUE%2BtqtYlKk27SJ9%2BRN3NlwApPRnHoPgeNj4Oy4A9dsys3rDAGA1v1ooTco8sOxfV0n6okTbwvLEHoKolilpCapfk2rkepNUqOP9YpVdrZlBVSQ7b1Kut2Vc8DQHVmlovqRw%2BWDH3Z1PQhauymZo6C%2FM%2B0sQ1qj&X-Amz-Signature=ca8e79e75363aba33490bffa3dde4b77a2b9296b992133673bed5d269a65c09c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

