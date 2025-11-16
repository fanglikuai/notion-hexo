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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPKWOVDI%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB2kJXLTs8ATIfjFmMV4Qb3crn6bAU%2B%2BBtG8Yjs%2BbT6%2BAiEA4gRpo%2FHR0BILkt%2BvnBs%2FyMh3JMkoEQhlPAAphi%2BXnL4qiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjZjY7WfqiLVNrNPircAw8dX4yTjtfJTm1Q8wKF5iwNVZ1oc0%2Bm6fPg9UaF5V2RWVKP%2FFqmdm%2FouvOHhknMy1ld5DEqH%2F9VXGLONlyEsNsJbfBX5R%2Bp4jwv8YC3lzBFFxk2iG6bi6GujsKYRwenAi4cJSadFX4NIc1smLK1YcHhwJTrtmQ0OxM8DJuSEwpb3BZtCVkbbPXFmr8EYAVasTIJc0X7S8BhvY%2FAI1MtXLlgaN4MQGWAZQbioURUy5YLegW63nyP8GWLTPLka7s5FriXrxU%2F%2FrvRUHDvvPYrJo%2F0IoQSA86KBAJhOt6xkviQZsqjGQT2a1o5BNPktSNojrf3vpw4CkWgYi3QHj2VKUNPDR11AJYihI%2Bqvss%2B1UlRKqYuOQgwlKhLc8lP%2BGc3PWRgl0OQ1FE%2Bp7X2PBBIKbhzLmh33zn%2Fp8O3vF%2BveQ%2BdJtjPwJCUqI9C05n5rKq1Ilnax7kcjKO6%2FYf%2BQq5bsSPgjslJUxN5u0PiQB8QWtRPP8n8CEAlaOhneqkBLoLOfPgyU3LyIdkxXzn%2F46SaQLOXiu0NzBnEvQMDtb0qqLPUGErye1gxzqNuy3Pwmusdhf2W3hD6ZaATa232uYRe2klHtvPd25EBKcSaJ5b7MbDKRRG%2Fhar5ghtEMzk%2FMOre58gGOqUBolU8PTyvlkqY1ewI5Lt1d%2BMyBMyDRgWmnsqxrH6Lzhij2w4om6JUftVmGfcSPf9eTKm31Xkyjx8fjv0IXdIOXLcZZHhRu599O9fTzVCPSmFjb%2F8afl%2Fi1LMEwIXsH0Fr0Q5cdW5Urz%2BOcdWLokx8tFcQRJlonTI5CwITp7Zs9kKCNVNRTVAZ4bKV1oS7jVHLvbBowtkCSSWkhl%2F03fxubBMFYCfD&X-Amz-Signature=de36e230e30630aaf067a015111a3ed8e44013cc9ccb0c2998f5863bf3596114&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

