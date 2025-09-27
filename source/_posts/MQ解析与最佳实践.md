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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCBOF4RF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIB9we0hhFrNU0k1fWsNWV6YXLuufXT3CMvJY7%2F4h46xjAiEAobqK76bqywEGGgczLngXSy0DyhQjvccgVdPX0sQuCysqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOalcfIHHv0u9LTrKircA1SXUVZglrDatbb6ccZcsdwsiVc6Lp4Ro8Ddq3NkulgqkxnyUNsuccKefR5UyWoHbaDHHXzjpcKrIoad9oCMUeSMB20q2MVIfS47lueM8t3Fv496WbG2Gmq1ki3E6zUiX5wJgRFmlJ4MTDQjHsGuT5PZTMWrgmeT5TTsQP3634bGeCfCFv4mw%2Bp6%2FWDKUNixcqTGKLExLqhLefG02GGWhRqkAiT9sbMCKhWgVz8ZYLHR8hcMobHjRNSNwZbw3N79I6gTlP3NwgIEiJAvkO2MdHkFJg5YZklO35cUIq03QH5APMobBLcxqCIWboHNBwxdVM5TgYWeSWK6ALwD63RH1XsPS%2FrE6BX73e47AAdTKJ0B83iCtvaAqfAq9%2B%2Bbg5fxidl7o5nI%2F63vNGGDy4nHZZ3eA0eAL0gpPxCHpGhocwM6YhxWMheV7LJcFAnow06A7nJJ1DeLNFMZ6%2FVAr66v5Ak94iw0tMFowFzSgv8IPTtAUqxuPrBnYkqtdPkhRy9v9AVqM5VIStZgL7tnRdV66KzsVg%2F9RykYZg%2B9miS9bVSBcsO6ST4%2BaWpYmSCYFJy0lTk46f0y3bq0flPN3YLRX24GJ6nq0NiQXIgYse1GHTUPzns1E4EsylBt2KzGMJvx3MYGOqUB7lStwUczK9t7hZ4zth3kYCWtnnJwUsTnUWTN7Tg1wSsY4p8ujCWIwHyq3LEOP1Riu%2BToXZt3jBZcF9hcg45gG4xyqKhoFaHkXegUvgNIyDLiaatn5vF05J47pF%2FSYuQ5Ykb23ge5GGxXX4Hs45sKtS%2FM9JyDgfgei6pJoCcbLQEkFUpUvirKXoQxNAdPMjzmHzWhIgjX7r3HIezzM5hqBqRIJz6d&X-Amz-Signature=e0500a2d65ac9996d66d349f9b277ed44086d2cee0ec4d25b384798b342f9f4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

