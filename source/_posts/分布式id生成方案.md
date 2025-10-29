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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663M2SFTUR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCyy%2Fak7GVdkZrPBs%2FkNis46aY42M2MsUeip9n0ECOneQIhAIvKzKKLDNeEx8SMnFlPfxe%2FspKntt7eAFuxUkiVUMSaKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyZRDg9Bzd52g9xMAwq3AMH98JWxsX4nuo45LiLxo7FCgCCChl6GqBeoMKPUK%2F5tkJYJWn%2BbT0v3bSbBHXarEp0I2Zk7BEFWhlQ5DMGrlEmOBzeBUtdS6OE5sArdXuJIbXrnqoFfBCgqrVMYyth2MAf4bVCTWc1QMN8t1JaESDZQts3etGd5M%2BvAEwkzzMnTqBk0kt6UAnlnCZBa%2FREjeJ3ZvOJrAgh60etcAa%2FAktjNqapQ3j4c5HQtwcO1IOC5cLIVJ%2Bq9fTZAM1PP7h1TIkdT6n5F6SrzMmjNybDZ4hiSNxBS4F4ZvsCBWcFu3z5N%2BHFB1fR1tPrCWlXjwPSicc1FLBoH0vP2p8FbnNkcH8tSjr3U7r7V3xE6cE%2FVLHM3iwL3DgafI3uIVlYOi8WCJX5iESnepe2cewlJj1tMhPV%2BdOenJHijHHXZCED5JkuEItyx8VhXm6QpWTgEqynPGNQOZ2Gu2tH2ikb2jjs%2BxvnUVqeqX5e6GxtnWTH7Rn0MaTpOn9zcsU2KKVqHrokBBc65n6rk3AoBWyN4vQ0l4ijOoV5eZda6x473OiY5nb5oUFxKAS8W2FvCXRch7sIRBT0KWz0X8iuj5g8h%2F%2B%2BAfYIrU3qTjKrlu4ECTE%2Fo9QBc3ERng1RkuKYetICVDC%2Bh4jIBjqkAa38q%2BiPuqlimf6IFPnWt3BICXOb7MaN6e7M27Vsp0bxC08jHv3caNNZPi9SWwdCLKe2nHogXHPZdtsPuWf7AOYsPmVkMtPTFL%2Fep8Ue2p5Z4cV%2F4i%2F39DpOwlW%2B1NRdMYK6dQCYv0o8y3lgK75OsO19j%2BKLJyICs6O3aXS%2Fj0lXT7EHp54yQKFyzCtkUr4FBi4Q0WD4bVP7DCLqIQRIgdvjjEP%2F&X-Amz-Signature=198fff120a2a40bb8ad27a342f3af3e048f9746b573879a95ad45341e07807cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

