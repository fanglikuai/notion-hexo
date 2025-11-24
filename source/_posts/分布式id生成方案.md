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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTWXVNZL%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGYpnGst8h5PexKKnFCVdFca4ijajzcJ6if9K0rNistQIhALQoZmgJ74S1ooiI6risX%2BOhBNhtIvtZ1BlcBvXIrm7qKv8DCFYQABoMNjM3NDIzMTgzODA1Igwl9YfwvGVK%2BRLs1Hwq3APnFakDmKmfmxjntL4GXrktHzb9y%2BXa4A1Dy3PIrquxT4X5RUXoELv51K9w8SX0xAPccHXB6kIn%2BTfzIf%2Bhvauq58cIbTmZrWZLeVrqAjDOWXREd5Kb2kZgWEuPvdoO4p2lKBqPPrKuJWSQnxx5OWxVKfOUh42lZv6bcGkelQQSCfZCAtQjfjdbz%2B%2BtNZ%2FVKQLUJsJlaGZshAP7doJ%2BwRTNIu5cZ%2BlWvV28tn2c%2BYE2L5U7gVHqxSijY3pmiUUL1nld0RdFZQpES5a%2BqLaducB6s2SWOYSCZ%2F7fCvwi3vjK98O8JtXDECIzEy0EmRyA0d5MPz%2FcrdAjhQwSh8TQX%2Fd8wC1BzZhbD9%2Fok5YW44S%2ByuaN%2BtwITQ9D8%2B6c8GRGCcsJ0XcvMM0S8qPJWcSrf6e0OOAC%2Fw06ped%2BiX5cog0KRg9ESlrTIxrsb5tNYD5yGFx6AeK%2BBxE%2BhswwLiHCxnvM8FdVjZMtouivell0qmdqozg5QtjQ9%2B5LNH5koNWnyCpeKxkDbhM1cDHs5vaSGPPJ3XGDgVaMCdvT5Q730IlXAYGhKTVS1cJwEWrzh5knSXOtgCiynJyDt7iiNNOGvpyd%2FXkZY8xkqEGQTLNZaKKkhiqo5xBHg2u%2F8r2TLzDWopHJBjqkAUTmN5om0OT8C2rpnu6OlQ%2B5vKL%2BFuy72kU4y8ys3kAE8jgQMbbrH9uecYrnki%2FdAhDQoWh279op5FAOYjK0Zg%2F41Gix3ocWBhUXO7m%2FatYNXR0z91tys87Gy6pdnkRRCRH1UZd5VMaqpfIaasdhKiP2eqa79zQMGp0zsS%2BMue2jkAmlk%2FiThx7zjxKrf71dvVssMOZQQynLIzwmgFwhbpCXbtrq&X-Amz-Signature=608b25fc6142365d893dd4d7bd510e8bd16229a96c887f59f162fa6f77feb8e0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

