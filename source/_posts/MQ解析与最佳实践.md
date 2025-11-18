---
categories: 整理输出
tags:
  - 分布式
  - mysql
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMGIVV5Z%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBrfDqUC5G16IhIT%2FCw6%2By7kziU4ijaLGaoYG6%2BSJR1KAiAukL42ohyrkDWy7ZWSrSmoj2Cdhlk7mB3gyCWQO7xn3CqIBAjD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FaNZonTpVw27HpXsKtwD1Taw3JYSrzW5AGciC6cYHv8EoEb4oqlWBAxMcc7wGXwgKl5Ea2vpujbt3%2BnjkoayBO%2FHKalwNQrWd36brdBotF7yukGCVqNlGHgMdtylAF13p7QfboXJTwhcas%2FOpMQ8Cr%2BI68hXGNWq5ltr%2Bn3s7G24AGLn17V97hIv2Kl7wA59IoJU5Md9xc4ckL08KHw6CV8jKRtD4%2BxkpYq6m8RgsNaDz2F0Ab7lZqRdM5Vg0BHrhZNscA4K8CixwyYfAg1c%2Bt612ruDKGVYUFUIR92OF%2F4gL4BypKqkg%2BiNFskBs2GR4g%2BugaEpPAoKSIPG7mK8h%2FBbqW%2BVm1HVVVu4oGeerndZm2v2D8k5rhW7jPYoA7YlfsJOWuNvBP8%2BzKntNVjLDotXU%2F2tnZPxi8rQiW09QpOgS7LXu3rBaEAFPdw4ZyC8sNoUEohsD1CRMzeViME6k8psnU7%2FSySSo4eh3B%2BaAB0DQX1BHsITMe%2B%2BGRrcKaumWxZ1mkUPUexySl5g7hxcuFedtu2BXb377IOOWTlvXG37x8xyj1OHoq17Ysc62tY%2BrMqYBUq02PFMVz6L3EaCV9kW9gu8QSV5ZHi5YvXBbsECCD%2BG7NTV3vqRzmjJg3O5PAH6nIK%2B1Ze0v6Mw3oTxyAY6pgHxB%2FuRsCyliOQulyT0s6GoiAL%2FNnY3PVjPQqhx2gOkXvXCimsY7VBKgGl%2FvYje25oUIF%2Fm4Jd8KV4MhC6hTWcgm7A%2Bp8X37xyC1FLonG6a%2FsmoArJbcWvcEJ62I%2BaREEJRPMWKLXCdu3UQeDkm9mofbIXdIPRJmjnmoRocfFM8R94ZNMdmw6VYiS6r1z2kXuPBc2sPYLmRJT8qyU6orL%2FQmFTOWntG&X-Amz-Signature=a04d07d5ed5f1221b73a178850bdc488f0f624c94fcd7f1bcce42aeeeda8b423&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c8962001455e38177108499d1e1e605b.jpg
banner_img: /images/c8962001455e38177108499d1e1e605b.jpg
---

# 丢失消息


## 生产者丢失消息


使用ack机制


异步监听：

1. 成功/未成功发送到交换机可以触发一个confirm-type监听
2. 交换机发送到队列会有一个publish-returns监听

**但是一般不这么勇，成本太高，丢失概率低。一般都是采用日志/邮件记录，手动维护。**


如果使用定时任务那些，成本太高


## 消费者


手动ack


应对：

1. 消费者失败后，将这个消息存到redis，记录消费次数，失败三次就直接丢弃消息，记录到日志数据库中
2. 直接false，记录日志，发送邮件等待开发手动处理
3. 不启动ack，使用springboot自带的消息重试机制

# 幂等性问题


原因：生产者没有收到mq发来的确认，后面本地定时任务把错误日志中的消息又重新投递了一遍


解决：


redis中增加唯一id


# 顺序性问题


使用一定的策略，如取hash值等

