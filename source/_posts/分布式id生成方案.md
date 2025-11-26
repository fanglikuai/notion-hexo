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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GZATYLI%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T040054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDtos9SYJP4ZxUGU%2FcDzh6lVIyHvUepfzbHdrXXiaQAiAiAlPyE%2Bs5C7cUpGcrrSsVkhzTS2mqHIra1KZwfKfY1mgSr%2FAwh9EAAaDDYzNzQyMzE4MzgwNSIM1IDm6n0DoxsfMLawKtwDyHm%2Bb0zbo8K8SWyoYTpIS24E1M65FypwvlIeJOFEyk%2FZtspTFIT0R3sQ4KXPc1WQxoODHreiXD8ciCrBhbFZN4qojT2MQTYL8bIf5BSDt9NLymbt8B2qLoXr4z7AKs9eS4sYXuqfhaj6daYOd8i%2Fh50dzM7UdhqWwaJAZyuDX0Q3yUJ5pKJ5plTpFufvS1ilVkOjTmcqhhTrrDVUQMKcqUiTZYyCqjL26VCALvuHddg4s8HyXigLAqkQRezlRkRds%2BpEjza3nmONwW79Iz0NsoaqBff3NH2mGo%2FufGUCE%2BLHgUQf06i1QN8sdxa1%2BCmmDz2IeZzkyEInPZr9pnWIaRjjprAwXq8%2BKsP0Mrgg4XkRYmypirlikOxY45G%2FIRBhGdTQJ4aNZ3%2FMQPDYtKEiAr1YQvrSpK2oVI5SN1BwzEaHIupdd4T7MnuJhOWyShVOKxt3xbvCHh2VdZ3OTEoY6g6XEs5YakTr1P0Ir5KRrOkMXKb%2FIHjwn7iDtQGWejcuVbMnKQuLFnpib8px2OwK%2F7UUMImEvvd1B5%2Fh0H1ejNPUfDngQdRkiXiVliqRZ9KzPlxBB3aPWWU9sDll6fcTsZ4YZWMtxIuTSoduR62YwchTaGwEKtkrIUJeSVIw1uqZyQY6pgEn8nsu8RCb6hh8DnGdJby71DmSXnF5Wz9QJMYOWdhvmoT%2Fmm0bg4ftkA%2BRaQyBigUrnSsJye84MJyk1xydv2M0p2%2B1%2FZMq4YT%2BxbVPSO0R9hXhCwBsgiykZ4o9Us%2BSBj%2FUwZCayuj8%2F1bnHcO8BuMhVcTalgKI1iaxs7YYArRASfLyNw9IkrUsvkVWWif42FOVHMgXY%2FbpnCXje1OIdSlmSFUwG1Ub&X-Amz-Signature=33e5f149376b16845e3f55becf6d4551d73061523a492656089c700dc558a659&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

