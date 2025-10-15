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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWG7ABJN%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEr%2Bx%2FAbCKzez4zrWs%2BlXQKuXmDFjKpjKg2keg6vgww%2BAiBenvqF21BU4TRDMifgv9lotxYta%2BWEIfjrgH7f%2FiVphir%2FAwhqEAAaDDYzNzQyMzE4MzgwNSIMOzlqRAm%2B1T7XvooxKtwDGauTBNGs93JQqvpurUdmG1%2FjZ%2B%2BhO7MeLzVdI1NVUEhoWHhD6dZ5HhuN7IrAZNpYhKYi45e%2B2iHaTgQNVA848rmvbCcD2uIr4U7jBiubPwPClN79EfonBKNiwWWGGiL53VeoxVSChw1OrK%2BRSdZ0qB9oGcWTSxRvNozMS5REibQtkQxtW2UVGhgV%2F8M54gl0nl13mTmpToNcJIXv3wXPq9AdvaUnr3avl5z2INhC5PLRVLQ5ubyN2R9PzwZPGRUTtLpb%2FnOxny3sakIKqBP%2FPkjbP0ZS6hlkjJaswWTK5PmjrZySJIRCFFPS0CAv1ZOuTPjbyHloWNVYk42aRTAQvmUH04re5T%2FzBM9FpWHlVPAVbz9KivCnIz949OsFf9hiZN%2FKtadhuEpPpRtUpRzwA7FbrAOuGiGzJ49d%2Bhkbqorf8INiy5a4VKqTrOvs9oPT%2B%2Bg7zRxfsnOSxpIN0wHPKomWeqGy2nW%2FynCxJsZs9M4OE5XrGeprPwzOfDbLuKOgLG8Up%2F59wjmHZVngZbu8ZxE%2BawmF9ySzVW4yeKyl5oA%2Bl4XhlEGtgeMwU2tOkBw52pL5eNfuGI%2F7WGtmJIIVHU1KzgPpRp3llZXr42Xrj4mcjnkoWpx2qjLLOKsw2ua7xwY6pgEY8WRbCUTpony3utw5f88CTsCXCGyhPyMEVlNM0iCxL%2FKk2HXg%2BSW8vg%2BKjyNOlhf68Rvoo2HWRKYaaUW6V0DF3pKlIlmJbc03fV1MCmTUoHkmxahjnmTnFD8AQEGqhgCVuyIIFr%2FeuxFju4Ylsp1kdJrozRGspGELnT03pnUEHWBbFBdlT1PN713hK%2FPU0OzKND8t7%2FiGACx2X%2FdT4w%2BKhzUmxlQl&X-Amz-Signature=bbe7b1a77f70c292b3af323db32054d7fb25eac5bd8372296f54a670031ee7b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

