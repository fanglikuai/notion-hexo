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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLPEH66L%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T070106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDprYvx59Vj7b0gaZMq4ENc3IvtO8LZBucHVU4brKhihAiEA9UaN3uvYriIuyBDv4xyQBSqABWOv9Wd8RVSjHwpEfdgq%2FwMIcBAAGgw2Mzc0MjMxODM4MDUiDArfzUXnrc5WTgUlRSrcA3t0d06EDwuOTCscTRfdeY4D2WkAb5whjOH%2FlXQ%2B8ydtKoF7nmtjmBdlvudVczA5g%2FM%2FnaGMeLjukpaPx6HWy01Sp2yrmk8%2BDIzaatCVpg%2BvvyGtJKE1OoUzqXAnHfX3MsHZlU3kRRT%2BCTierY4%2FNds98BVnvvoImc%2B0hOxgl4ImYbJtWYhqvmUxpyNGbIOIXDG4CKLfeWj7BS%2FN8R2TXcFpD5n4IW6NAEKJoZKPIfLXoX2CTep%2By1QiDoOSXHf2bSIxoOEnZKlA8lJgv6s5xnquLOgAi4rj6vJXtP8pQNKEresUwtTpfdP1PhwcFOah7a7nbhHwA%2FMXjj0dvysYlgnqqMp0XHGWtaK7iZRLtwo%2BU54Dv5obxNJl4AeLcNlODRztrjRALfwWV%2BMtD54rFbQyuK2PvLKzlNuI2ZQNbaUCDfvCJE5t7qaxba%2FQTlvla1xttEISzIwQWzSIEI8IFvuiXLj6o1rCgnTvRJ5wh46DS8FXbDzi7N9rk7q2fYO7pk4FcmYBJTiAPVwBNTv0b7YzE5LG%2FV4Q98PWOWdLM6p3SJC4ouTDo5evMHiHs5mf6tiVNJukwFQiIlXTLOylKR93PtvGKcArEUyiEkFujveEXyeDAi2rlub56Sv3MPvq8ccGOqUBleANX2a%2FHTqWg4Qe9sVRKMLFYisMzyWpnE%2Faf8PLQ6RbhhQxV5VUuV%2FOlNKr%2FI9%2F8wnf4f5kpbw9r%2FdQ0FCBPENBfHEZEk2bAfW5nDkQz4jjM%2B3SO%2B8VlG3YCdziYhzxIOYZg8sio94ACzRWH8uSVngGL%2BE3373uNvOK1Ft7c4lv7OPO6et%2FgH%2BkbqvBXajR6pRpW9k32itrz9TzJ%2FFkoOmf1WLa&X-Amz-Signature=ddce965da2ec7294876b48fd532c7a24d19a97014e6579c02ef2382c0b4c8454&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

