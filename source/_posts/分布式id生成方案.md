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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V46OXVUA%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQC9RjdydfdzBdIu%2B%2FnK4CRmoC6SgbvVadDfOywE%2Bx00YAIgY%2FB%2FuKOol8h3RdOJdo6NgTRNdV5Ztt4y7d4awnbv5Acq%2FwMIChAAGgw2Mzc0MjMxODM4MDUiDNRFDuCaOQcE2OBBgSrcA1JW5wk3e3CFbnhP31ppD96v0yPyf47zZX%2Bg8rKjAJpqWBpHakZO%2BECbarA%2FjYbvIKBUrrF8Eii8gk6%2FbiyWkP%2FR8lXLK%2BPPgMMv%2FO4wM6mA40%2Bz1LfZE5NaFsR6pxH2%2Fvu1aJZ0hCHZiiPkxbz%2FLYLCdyFDRh2AqBejiLU0qmk0T5WNOFqWx8vAptFgYVfEJ5LpL9sohS1q4UfKskrDz7O4%2FKGEDlqyObh5AWf7D%2FqMTNl7Tsb6gsREI4DcsthWMvTaSb5A4rCbqoVOeVxgLsJ4MB39PyMLaWgSig9qBN4wOGBBUlRBP8Re1I807C9Vrbt%2FFdwtiitYc6spf%2BFJwI0f4LVu7MeJGmkUd0zpWuJr93wJ6JKlKIJ89VrsSKUccNyfhlGIBoA4qww%2F8YVyAhW2E3GHpzi7I08lRhpQ6JnxKqbqpBLVphMu2I3%2B%2BvyheCH5y%2F0gYYB%2FC2BVVMlqLquACCBNw6H7BkR3H2Uns%2F%2FsP0g6oGIVrNP2ujXSelacvT37i7j3KorTYoiCeZAzdBYG%2BGzWYLQeB58hdMijUUD30n0H4%2FKOVQGv4v7YTGbQ9m9GRCtYizbBrA6ku3cBCEjSuiomfQfq0dcePyFaWWY%2BQ7GdWAb4Epx%2FlJOTMK%2BryMgGOqUBOtSocY5tqQEw5u84JOJhRtQhsuB5Dmq5w%2B6MP%2BmF6HbvMw%2FQ%2BgJSqosaN6p5IF45fPPQMivpvv5YEuQjbhkxEC6uOfuBtg6ee68Yo%2Bd9EYM9hRK83noNn56kqTzfxJfCOWCByRy%2FuBWBTH%2BvHIV3sjdudv9Vmd5WiKi0NqtK%2FMOwEkS3RyHLF7g%2BgE6fIS3fSOOwPffnD%2BbvznjABgJHiQEwLFxW&X-Amz-Signature=157db1b574e24af2f12b6b1a4c029d0928193bd58606f1ee4264f0db569e891d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

