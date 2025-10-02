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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZWUANZ4W%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFUnJW8Zzy5AMDYTCNLefc016%2B1zWctGzyHFWacsj0%2FAIhANq9V7YOZpIkvHEkX5%2BSsPcMA0URbpgLBd%2FYOLJvYKoYKv8DCC0QABoMNjM3NDIzMTgzODA1IgyesaIB6cFJdGOpQYwq3AOoj5dIeuEwzGb0FGPHcKsz%2BdGMTbJXYO7G0kYHL%2B%2Bqbbqlw%2BgYlxKL1buqrz3tSBAha1a3RVfOrldPmvN3wVUmPJMrSttChj7Tz546X5js2IUOpbYJNVaGhNiA88X2CvyTpsnn9S1tjlBtMlUgk6k0r9s24oZ6%2BxOYwfOe%2BahFOwTRSLIZUGQerjalVjxr3z8eLMryYaKKEHhkfxADKyDS4tNBhwl0Bp4GpXlNs6I5ogH8AtQwvnMztP3GO3m2uWYH9HTUxSWxRKzfVNqwm691ezVE4e%2FhN4ZFqELvZ2A2I25ox5HsPZg24c8b25T%2Bloz%2BSFgdPvEYeMQEVtQ57tKa2oa6wcrKlwCW%2BG1i68CacIUv9dNpLfo%2B4H%2Fa5pvJGiHOWuL7WOzIXOZOiBN3itUNNzM66SRt9ijxjXaJBFqt8Lw6dr5jKTOSpHrQUTW1YcitJhbmsV8YCiYy%2BluSTkTq1PTh1B2CN9CsdTD0iXdkKy%2B511CPMB8%2B5s5Ilevg33jnTDW3pV%2BDVuwDjkik54ANFdGcxhLe2pk5Z9iIdpirLzwNweQJdKAMCaFCroB1D8i2YxiTNshCljb6bgflP00oPi0cg18GxJ%2B1chEJzZGbQdK04kw9J7QL4WRiMjD1w%2FnGBjqkAVryLpi8mcV7%2FpG0swXeQ0935WyJGZ21cSOazmJruhTxu4oF3WNWBDOvcila6xx4RYnzp2VrqehfdvgpuHc92EQ6aLN7JRg4IA1r5G4MZ0vsXU46b2X1IGCddwaAlSTrUbdsAjgtRExDrQ%2FCaQdlv1NVXY3qbPK8H4eqLQ1hqRH8PQA3kL8qLfPBayAW0vUG0bEglz2szRo%2B3Hlx%2FVh38ZB35yxK&X-Amz-Signature=25c303ed3656a0a372427daf75cef22842e914b4e02f27a408a0b1353626769b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

