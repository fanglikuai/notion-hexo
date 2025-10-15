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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665764J6DM%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiapl4dVjIgfu7dvqJ27HfsFVOIQjVmdmyxiE2JiBclwIhALWek8%2BYsQtebeS1OXmOmboa8EqQsWFXtPRt0cLnlYOxKv8DCH8QABoMNjM3NDIzMTgzODA1Igyxy2bXKPMrt8Q5N6Yq3APP4tBgEgk2wHAO75GhsVBT9F8YG%2B82fmwXEAYTi%2BWiOMH18DWff9J7EmCJCBAD2gzNR0GzZsAAk%2BkftInuO8tAlleTQRZvmGXxh93QKs6kevJFGFDrKCALsvnkg%2BNN6wQk%2B%2FJE382BTBrYdAEm%2F0uRK3njFA%2F5Qlru4ig2qeZ9hk544TOuCgKMxfj%2FhE5KEbvgMVhHUgmCv7RDkRKm8nMtbpiCh9GHEN1A6nXxen6UmIkGhGtMbXL3hu%2BCZNbQU0GYjL%2F9j0DOQtHtNKry7PO15TH%2BRdeD2ISR6R%2F%2FkSiJvXXdfp5tB%2FaL6eCztp9XkglJS7GDJXKQeFL0LBMIrcgku7BQcWzET4%2BYTRgX0HLKQdvGgqROVOY6YszjwYXkKL7wmvc%2BKZCfa5pAkegslPz8zeYoxddiHEbmfXF5LIprhNXTvW5Ybf%2F6%2FePcPNgnHNpnGcauA54nqrsx17zUMm%2FihJ7yD9sOdKDWH3GO9Z9dAqQzSm8%2BfQFV0x9XLemhfJVo70IlmpmFp8U1qNk6WglVi%2FcFZL09JGT9daOlOJcOhu%2FsWCvQjjK37qYUDsZ9HtrX4yYiaJMGG3ckRN3ygE5nQJCWXBQgNQ818DRnyTZWNcUvGs5lyJrCeEESkTCetMDHBjqkAd2ohZf3YHI5AhwILAvC7vmE7PbYgTlhMWW9T%2BDUIkkw8GP6bKqeJ%2FJT9G7KFXYMgfrom9OhwSa1KQoIzQBkTJUKHE4NyObdlVepauxky6ey8eOlhfl7H96Ytt9zOBfvQAaa2quEeEZBWQPflkGcnzkLe5nGHdHyRWqDwOKE4cgCziS%2FcDl7ZWbGxoAUC9UjGJJgYJ9kgUEB6ARrq%2BaWwsDOcYcl&X-Amz-Signature=249446b8491edbdd19bb941b93886a4eda9ea6ce8cd7850c4b4a83f060a01f84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

