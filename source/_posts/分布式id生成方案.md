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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VNVB2LF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIAxN8QUbT4tUSZLlEj0%2F8SvFbOgoTwmz6TwIB%2BlWAaeQAiBvEjCwYzeJdYn7oUWgWv%2FXoqf7hrjXZ3LuoI1VS2HN9SqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSyfkMOupWglOdat6KtwDg0lfrztDEYzd3QWDnNwHJIkwiyANWXoG6phmIGJf0beehrL8ZpQ8rkO08%2FND5fw%2Fl%2FqOPkYssG18cIP2nW6KzT8tXJ5nwVYxPMBpj%2B0lPKqvTH%2BS%2FPcNIJOjCDvPAl00tVvWGoqwvW4iBtA19XbAqlj%2B9oYRlXZReSB%2B3I97hdgL1KcdcDuSBCLV3H2C2COcv5uYA8egqZcfAcTIXQgi0%2BENMLimNmT9IEIRHZK6tfbjItSV7J15DA5OnYKPmVetTfjanQpqff6jAZ0UACfLIgL2YfUBQa0Ir9EnyuNUmlX3eJ6TpVv0fNZbS%2FoDioK1uA%2BL3uePb1rXeDlUcK3FYlWts0vPVkbxM0GYR%2B7wypCojkvCFrUw4Q9YJKkPsBWFkkB3Xs7T1JqQ6LFVHzClBcHuLfxrGMRBETOnASWje8VakSHZSz07gW0B0OyCklYXUTyGeQ7FoxF4v9Vlkb8FwHicqsGaA75u1ZToeKduhjzYM9QV7P6MwYLnpWpc%2BvSOZLqHmwW65vjRvViPnKcYNSBeQbt0vh17bQcDDlnfQMaN6B7Jr229j7tI6JN2vgfrHDo9kqF4oYlJQS7lmBYBoqk0NzUFdGlPqNKuDvYTzg7b%2ByrhlAZ8tXk44pUwgKvnxgY6pgGYcCgwt0yQPB5uR6anlg8HNjbMTrg5qcaZZwjbC%2FmJspJuGcZ%2FSllA8WOU9xAX5xPabmmjm87nTppDpTvbkac%2B3r0QoQIT%2BzUh3zEv77K8aVPXSlHYuhdFHtSgyFZvVhrSOkiPDEjVEygg3lIGHl%2BBtOLWmGKLDQ%2F6SNK8PFINyrMErCbeiG1xhSbrgQT%2F4UG%2FQIBOXCZ3LJ8w%2FM2i3t0UD%2BS3afvt&X-Amz-Signature=923d79a1383fcd8cba6ed21c5828fdbf750dda71005bee2e7dbb77bb5d03721e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

