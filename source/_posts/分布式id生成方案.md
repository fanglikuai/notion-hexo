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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXW4EM4X%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWNcDPAAUy3sXWCUctkM0h56NHEIH2LCqm28r3C9MSuAiAPfJZJFNFt0wgEZ38w0QjL2TRn8MWxW8kW4D76tQL8lSr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMHrqFdpj1W8%2FQpES5KtwDThj5Oy6w5KIQwT7zktTWVEoRYOSNXPiKJ1Ziwp4L8XOpAoezVgGiWeNqwEukNIo2FzuU9IQ23eWUhKZ8FuibrVDvaUZtXEfR%2FmKPU9Y7Y1R9j6XgWO4qTzD8Dd6X3AGijumAVpF6hPhChanAlIfU86KGEf%2FCixtfpXUstd%2FBd7%2Fcj3ze5HxK561pd4M2pIwXo40LnXYX9Glero%2FZNLDRkEaz%2F%2FZh0d%2B5lHcUgr0wZIs2DbboJkwqRU8uMhMhGPicguDe3ypgMQHIbc9JgLPQwbfLTrVz1O7zLYezItCLR5QhlcPksk8ky3vyM3OH0PIEDt7pm5Uik48FePyPURiI3xmjXaiZG4peIbUjVCXCl%2FqQIO2Vt4dl2cUBfAvM88qC5wyQulx1bYgDrHiUnHHfgnSZ%2BHdWr7v1071%2Bksg8xDfS3si5kYFwIgBz9uviB3ujYLIz3L%2Ff10V%2Fjr9jCvV3M7LVxjp%2BeCpG3KDrXXI16f9SkiIO9kTK0y%2Fr%2FDFOdv1LMHYfiJCVUE0bNGGPv3NevOHvbAlJf6paK8kMxFzjy7PaEsOmw0omlcCSvm7niSk9I0SUsJDPLuLC07xhcdEvX07hNtIt4AZKByexCJYh9Ddt0Hau7VL0uXAWBCown5rtxwY6pgGA6e6quDo56gpyGvFZpbX5X47f7Fz9DKHhWIol0K7PgHaREIRSM81M%2FuFf%2BBLuXxnSagMKGxAa9luoGeNXzclirCDt0HqTu8iOljwqDv5MV5LWUACr21tDwsAyKvqdpJRoEH8R2uko9XMp69kyblTeHjjFbLR3o7VYyCXcK0kdDDW5br9cdkYi59Ft9WaLVy1Z7sa%2FeXKnRVguoJYAOgXLXz9pr1BZ&X-Amz-Signature=af75f588ac0b97b6ad20a0ffe32afe68040b123a2a94a5a6a13f20f47c68dcd1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

