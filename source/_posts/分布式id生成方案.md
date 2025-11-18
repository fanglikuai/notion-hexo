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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664I45ORPH%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDhprBeyNA%2Bbv2319r%2FaPL6pR0ee8%2BnTx33dlP%2BNv2JpgIhAKqnwTKJ91MXVYpdksEfH8jgnZZqMzmU%2BTe5BIPCcZNcKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMk%2Bij14iOCcs9c8cq3AMmMZug4VRimcfRW4egtFz%2BjJx6a0mkYrKIcnmIG9to8vKpMBR22w2vEdxXHav9gcvP%2B1lDNpdnyIpokrVjh6z3IQeYh8weqElHE1k07%2Bbj1rX%2Fc72XpEqQ1lrcFTbjfUCD688qe0ppDN4s7ABh5dJRfTgW3Gp%2BpLygCMrrGbhf%2BJMUBlIodPcDmQLiN09Fiu9aqFWInv9jGlG%2B%2FuasqO3WjiI%2Bl5p8BjpMf5OQHzzHAoQam3BmUUvW9v8fpXGEJeYJOXiZbu%2FgLEiZ73Oaitatc1xjWd4rVB%2FiUlt%2B4tigb%2BjT8s6Bx6OZIL6t2AU5Q0%2FELDbCM0iiIh9%2Blx9e1WlbsupBOe79k%2FZvcKZ53XdRpNQ8UQLO7UcI6%2FfFMEk7CKgyu6O05j0EBm%2FKUADEzmOOJAkGozfFevcDvnK7m5ET6VErYfNE2GjXFL2swu4wceS%2FmB3S3My%2FcSqxzu%2FkN3DGTO0xjG3%2FlP13Aoo4g796iMauA0dVlwgCmqg11mBedANmGPZYaHcn6htkZD2sbqgsXNS%2B6G1coFP%2FRNyx66tffF1Z5N85LSYruvb7ZL5F0TM7o5i%2BfnxYemuVY2jejnNH1vZHGHCJ0tgMN7Z69%2FBHNFYJ4PRQ6FQ9rNIAwjCOxfHIBjqkAXk8IVqfh4hAOsGIRDsDT5AAnM5bFv3gkrqXyzOPwJ0X6AOP4hJ11J2eAlrSeakXNq68FKN%2FFWxy%2Bkn2EbeWp9Q%2FSTmFLJlz8ZfJzG7qH4FxBixoSJs%2FqSQQenMBIVCqT%2FmxOb7TDTqkOSt90sC9s3k0o00WLruNlMCe%2BupnRfg3e7v1a9SKiTq%2BJ3rBVf1L1CAOQIh14yNQ7MMxZdU4hfmRLoy2&X-Amz-Signature=f1849baf2dc0dd91d13649ec55808e8601d7eba47dd85402607e9c3fc444d942&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

