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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKZ7B222%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA09FIxy6AfrpnEDDZyNhr44hor%2BP2GmlVMrbWrJ5PoIAiAI%2FBf8xcwO0lGCoRLlpTaLZDVz98w1%2Bf99OXyxiG%2FU%2Fir%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIM6eJE%2FLQPNiy%2F93C7KtwD3alpR9PteFMaKCXQClvvMKnu2oIIxWCsEnwyzFcoBgA1sckodkJ%2FSqrAVMZ%2Fqn%2BkJqBf7IcWYNEgtQwjS7FpHe5%2B3Ednt9tEgUFR3c%2B5dNzYdFX9onUTKNO4aWOs5NDotnJ64g1B2Y%2FwQ2Mbi4aP%2Blt1KZiIn195vioy5eYQUBNxqp0wiNb9IbkhCxskFw79FgVIZUZmioLwlse2S3H0acva2ZBsx%2BJKD6653HSrbHT%2BC%2FrsYa75L9V5tXv5JHc%2FvD0EW%2FmP3O3RuaohlmeIGZy%2BeJRFIDSxWsqulUnBYoRjh68q7YpBVhUWXaedUN%2FIcAG3KLCLi5ZhZX4u4TJtOu4cq4da7PFnhTpR4hyF1X1hkzEKIGe5iDhqotUz%2Bv9CrWdWPhlRefR8VilKSEJwS2k05Hnc%2FsG1%2BKlyGk8GDQ6LSdPJe2mT92jtwKpuvfe91Ji3D7GX4vD25XKLbthn1Jv7%2BZUv1guw3t0%2BPOgmQK8UPZgqwQL5HhsxZlsb4wHWFeGDMfZj1wYvmXvYPcH2lAcGuz6OpgoXLoDxKP0aFLvlJkuEgTFrKDgNRKdqmtPIneNiDU6i1i58gUXCpnJhXeFWZvVOC1YZRvEml3wDPAD2pNNWucxaTLN3EUQw17iuxwY6pgFAFqP7ZdWNWbKEUO8bvvwmY5%2FoKexnkr%2FCs7DY7YqX0NcjD3oSHTuKJpPsCD6ygjkPNE7%2BvgGuBkeMv4HV4PYglKTjFIcxNhyKpG8kdgS33%2FZmmrDXHxR2MajOmUaPnwfjEYb%2F4zM9TNxlJHn0OqFTf%2BHNqNImI26FkatNGCJQZG4JzwvPPV3dBG2u4zIkV1RN0jdwN9TAQKIcZ1DJhVqoeJ1MUZaw&X-Amz-Signature=2ce6ce33824cc0f4a4edc1ab2f8c1c0f22c595837216cd5159da61d42f90fc0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

