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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBYUASQX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T100058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaVX%2Fi7fkxofto%2F99%2F7CCaA22rZ%2BT3g51ZrUlcYPePOgIgMkfjn%2FgaTrEhwwAI4Ww2mAGEYwITQkuHj4ofwEPWDF8q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDLbWDJaL21ptxgSsvircA3HhALIq4SwrENfQCCpN69k3f4DVN%2F2ItvABQJwAGQ5zeUejIHN4DRLeDcJT75siaR3%2BsN%2FgIeT9HcEYAAOXrTwWB8heBJgxAGM0Q4mfj7w5tCAXYXPMzDYKXWTu%2F9o%2BFDFQoKMILhI1HpiuKG5X26i%2BzlpjaIq%2B7406CWgjI7W2iPbdSLlUlrbLAv%2FevRdt6C55nmB6uMWxldWQg3MB0EP%2BFwkEMecprmCgW5PLSGEMf1MZTQ6%2BsU37gr1PFyoqNTYvHZ4PUUwqGo09gqTO2V7FxOrPtgeDPDSNaa4RBdL9msZGlzQ%2FuaEEoy%2F78%2FjKsKJJBVgbW3z09WoAqvkEh81j7faoaLqGZs%2FuzyLS8Rn2kCpurutgtwLrJYZjYNSeqZ1mqkHuFA2B3I3yhgwXnOb3AvAtg6ZisLP54GVkUCUXBVmDfKml%2BjUYcbGoj%2BN0fQECzlyxdZNuFC%2FRtAFoqo%2FbCjtjjOFfn0ZHOxHPbSZAlT4%2BmHD2v5%2FB54klLpf5YfdRomoF6KlUrefxerDLJRBu4l9DdQssQL6GbyXRDgBmuSLS6uEFofqQeHq6ua%2Br91PIuUt9iYzamS5PiD17vpVF53b8b4hr%2B93tQU53%2BSRQ9yLMY8GjqvtbgarwMPzAkMkGOqUBlPevhsZtxuW8ifnzlvwwgkk%2BlivaJ80RrgwM7G4t%2FiTPaKAoVXBTOSJiMxAT3WwTfePYvE%2B2kLe80h1ngt30R7Qd0pqqSYClaYv0SQPxAEQDjSKs0AaaxrrQtAprfgq5fwKle1MAI%2FSqYBd7%2Bt41LBTLiDVVHbmc%2F%2FW3G8WbmS4IaCXHikUE8lQbJ9eYqmJXIKqAD3PN1JbIqyJqmT5CAr8V2jx8&X-Amz-Signature=362a1dd096443b8bc27d4daa9ec3d869f012a57e2c880c31b1b5d202bf168407&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

