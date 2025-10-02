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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RO3VNHZB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcwTa0ksD3BikxjF2Bu2s9pLyNSkUo%2BMHXQphScdPL6wIgUWtnWL0GYhOdtRiQ6zGiFOi2XKvz6A4MfkD4iQv8nvUq%2FwMIIxAAGgw2Mzc0MjMxODM4MDUiDBl8MRJzBQhh8s2gqircA3A8ybI9JzpDV07uBeFiX2x%2BqOML8zI2o9Q9Mz50VnncxWA7pkGt6xqT2MJOsGRwS5bb3FdK9Zf8lRAi7hL2PSH3w7iGyQxO7TFxxVgaPiNZddRpedos897dy2sQk%2Bm9Bvo4eaHm5d54DIKuJf5SnxYwhqAo0wb40jhFcAxVdWUwW8aB0OBkl%2F%2Fvgzr0HSSKmKJxEzhp2r3G2kIPJIhVhVWcEdFqncuCw7%2Fqn2JIahdaUL4f76f02f8ISDljPARRhv3wVzXwlmJQtCScDxZk0UPFKwadwjFWK8Z9yry3Q3ff78QVi6VkcDOA5Y7%2BtOA8x1euKQ%2B7a5lRhdO6R3pSaFL0xC53USIGO8REzSluVsQHKkxnJS4vbQLuBjXN%2Fd%2BLTfEus4NLfHAhLHPWhxr2bkdjeNcp7HvlSbQpf4XLhi5NL9%2F75GJlCfP0vQaxUUJr6B21ETxH04UYdIW%2BHSviWvYdrjGJ8MCfWiSaJ%2BPrvEw6Wtrn%2BmcxP1j5H9mfA5Jz56YFZXxURAcu9p97%2FOu9wctmXPV2UAjFJYlwqWWgtA21Dc9SapF6V73jUtuc6sltROVPePA7UB2kEl5Vmmro3O5TT%2BycsveVcKLVqqLEU8t2iKKceOvU96FR1eMoMPK%2B98YGOqUB6cxFCDe9EJFHyHBoshEOB19qPi8jhy%2B7S%2Fhf7TTGW6D0NrJcZehrJajKB8VgIQrevY7RQu0%2Bnaetuajhv158H%2BKrDBITPO3Pk0K0C%2FrnOyZWugb7Pg4%2BvLSKwbamu%2BH36gXKbHcGOMKirylUfXhTKOX%2B80On7du7Yf7mvgwX0xdUxWOGSSiSEf5V7xMUjKhHP%2FqkYw7a%2BdcTnO4pD3KQC1rwDMLU&X-Amz-Signature=c899d2d0bcd3e9d08687791774df452ea4ed0f0cec9d3953146f7ab9022905c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

