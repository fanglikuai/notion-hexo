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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKGNCVB3%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH48UClHIk9o6uLn9aftRK5WVaiicLx37Czph7UWKZ%2FAAiBMgnMBiZo0MTgLWMNqZ7yj0qMYBHbnI22EKgOeGBi29yr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIM%2BziwFahvKvXSMJqdKtwD%2BMRl8iCT%2FIvOhP9yaGMiS5wXvoabG31TYAkSwHTw7PsQycb97mUbMGmJdQkBXcRdfjZOPvntTL1vTXEgx08j1aMG5%2B9bx94YA55HLWOsvX6Xe6gu1x12eDyisbFfqJ23tikr1NCrE6lBLb0m31BV5zKJM8gFwz2Bxy1XORhz8yqrQYz4%2FGNs4%2F2UYvBQqsnqR3K13mXyggZyZE6AHBt67g0zR5z70Aq3hP2C1XAdvM7ee4Ee8l7Fn2uBeVAVFb9js7L1ATWbKsxjJWOynSsuzjW42HCyWmiRg7BErLTVipYmqUg89qe8YitvG1L9TnfjiytDy7q5Pp0enyS7m%2BtWi1X7j%2B7OUb0iDnJu42m8jF2Cvj64ehBef8aPwUgbBdp%2Fa%2BTG0Zbb8XvR8rrFkdH%2BYPWLorZoQGxtgPfJEeN5yUnbPKD6pc8n1XiEGF6cBJb89sKlxPWd0HhyzrmqsWNDBBlE%2FQAJy60i6O3KOAyrAm5%2FAfaCH5hYlxJ0y%2F68%2BEKsgijqP1Dne0JtV0HVN6mKVgJWRrWgNqP4br0OU%2BTbW3DccwzUqi7sgghPOANb0Bpi7AzfaXQAz%2FtDg%2Bq4L0V%2Fh9IJ2NHJN5QPwdTsz3C7OwWDqjD67bEUWL8hcA0wiK39xgY6pgFEEiSpmz%2F03yddonFMt2tl0n%2B4%2FhrdXRb5NhqsEWVmGhaAuwzFV7OshlIyovzyOSMsteLT%2F2HSUkzz84VLXJzbPvV1AooW0KQW7KY0bPBE6THpf5F7Rg3KzQ2GyvT8F14gwPlv%2Fpwuul48ywgs0hN03JIPZwG%2B13rj4ZIZp1C2tRNi%2B5yZ2vLbD5MPlkROm%2FY3taqC0%2FmE7mKtGbIh1bv%2FeclI3tMi&X-Amz-Signature=ef4ce85db9b40bca3c79e2f310c12e339c5d4c71bdcf9cae006f518c587e0cf2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

