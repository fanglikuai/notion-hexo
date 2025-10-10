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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDPTS52O%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQCQH8UI%2BwiI%2FLrjk4SPxZH9CFoGhpiRMepE9hZHjzp6jgIhAJgQjxzrscF%2FH011Aw7imrElpZfAsh2gzbw4KLCtzwOcKogECPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwKOT9I9WSbTks%2F2CMq3AMZuAH0ypYQohYh1iLskM%2FfUdtxq2qurSa1gH22B3%2BvccuWGHdxMw6jqEBYlA5h2xAUZMSvN%2F91TTwGrpkJ2oeWRrGpTsBT2qJwHmvC5Tq7%2B2tPrOq8J6KNQ3sSQhqfcpv0ILTHu2GlEt0yII8yPx7nsfDM6XjJdfsvwOWg%2BQchcCwsTIOykx2XbJ1vlOfsBiEkivqR21ikn0GOOfKliKjcNgtEkCJn9AEBPCzfBAFDMkXN0uqiVfYUtduQVRKcE5JSItIIRMhAWk6V8r%2BjK9Pe%2B8HQaIrzCl0YRL1lHFG7gKb%2FF9%2BXvtkfM%2Bwz%2BmTqnbf%2F%2BoVshQvq6yTLqIJhSDLZLYvAPl4sDKGAcRX0jN2NXpDCZuPm7rQjGVxd%2F18mYYmwJBS13njqxR2jHaGkog8eXzzf%2FjSQ6qe9smq3SzSOlj3xNKxqF2s4LWGBaUXCYGKgbFyIFoMZI5mHDVQKufVj%2FFYfPJuzVRyVYYh%2BkJoqo44zd59T8UdSawAoGk8LuQXP%2BOKof66Z4Fl3JYvGBm7mT3qCZR0dPBQ4XyFZoG2Y0v79Wn7fubtO0BkQX1LiO5%2Bs%2BVOR6KBVXbg2bjDJhGbYGzPDBzTx9ahjHS%2BIDFGhkiWncw0GwPUjkWD26zCIvqXHBjqkAbtZW6oc3F08ArouNhrPda8w7um8kZ4hWWfnmpAl5MrPRuPzAi06zqi697k9zgZctIizWUtWekrWNaf3Tnyio3ZuPVibtS1RJmoJHaAAdmyKGbmKsRW8ATWfCWsnliJ4SEDhL%2BmLQpVZkQxPVw3kqlNZlUfJjzCIEiKl5zn%2FiZM693Xpxkj2CbR%2BAJ4yNidx1WP6h7sOt1e8zxk0sUDMjufkaMhC&X-Amz-Signature=be254a4739238d61afe69cb169b79d52ab05b217a3ef0153032c74ff169f7a17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

