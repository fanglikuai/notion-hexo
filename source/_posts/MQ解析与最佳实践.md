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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46623GEFVMM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T090052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAnTzovZI4oxQ0rmDFN8REjClj%2BGnUf3dUJE0E2CeEpoAiBzurCAFHDrKL62SwaPclPqroP%2Fv1Q%2Fgd1FFSer%2F0ByHiqIBAi6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMRPzoKjO%2FFqYeEda7KtwDmaLdt%2Fhx9nPhakb6H9DRWf1ve1%2FsCD%2F0A6MdKh0Amp8N4d07hiExa9M0G25Dikh3DRFcNQVQU4SIfuIiGCBOPPygzA8JEcYquIUVeshpMWIehNYm%2F3BYGxcDxoncAt7i%2BLsejq%2FHu%2BA5lDRj7EhHDstd0u%2FmIrx99M1OzUYCInNAiikIU6FK2gXiS%2BwBbRX7N%2FIYrAkVIsQLR4Ozlarwakv05aKtniS696LPxl78ct8P6j0fi1L%2B4%2F%2BhE9j5hOi0HeAl%2BjxlKo9gcj8yRyD9DUB0aGL8Notm3cwf5QdeY%2FQnFff9oKwhRxGyAFI0sFM%2BvOzKud7Uyy06Y4XJQNvxna8HYG04gfO7xmFBvMsoInvsTiydpZPPrbY14WhA5DpX%2F7dKQA3EDm5%2FJpEhqnh2s2eUg5YuOoki%2BbZOklhR2qsUJ3zwzUZoOoAy6qyXIbzp%2BXD%2F4eDYtY2ZfVVQSt5A6V7j0jVJdMiGDPUCMjKNrro6bWA9RZktsyySpCQXlTitxZLOo%2BOieXPQXGyBUa3PUvdpJay32%2Bng7YGm6ro2yPyfftdIc65eSK329jsEA2LQKahIiWUPVp2cCm3SeraaDg4ZLwF74Y5iDbq22t3%2FmUDnRNrD4t4tBWBe3V4w%2B8GYxwY6pgFHIISz1mazwkjTNZcgQfVjRIbIiKlx1dP4sdKi4csKdx7Nnwn%2FSGVFeIaK7gOn9NfHWhMb%2FJ9PvzbwC5uuL00js9OsPFaZZ%2Flr0qsm0nh6D%2FcYiaYh%2F6wDSpXRulausrVkjZ%2F6z2IkoMIaboxKK3U3cGxtiR%2FK%2Bv06Qnhm5BU8v9eH5pHbAv404ij3ihwZdd%2B1jp6zdzGO8JZTBfj8bwkAc2PocY1M&X-Amz-Signature=1c22c828d881993c28a2065b2512768e79bf78cc95703a296d2ef8172e0b1f33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

