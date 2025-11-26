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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKLWWLI5%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBGsc6slF%2FrrJLGoVeczsBQJvupiQhtkJjCN8v3DcEPNAiEAgCR%2BmxdbHGsgMoRO4HCxkA4xREDcdB6NBmbzWkGQ%2FG4qiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDABt%2BrydxytPICmZfCrcA60AzxI5anlO2vP866r0JEXded6JqMTc5rpFx3XEmaFfEJm2zGwjpedjm%2B54hbVEkLlIyEVhWUMqB352ADFpadzszkoONcPVjS9mDIH4Fy3CCXs1ZC63E8ybbObSuEZl4%2F8UtzqbeWXdBbmfopN0lA9ktzECukqBnPm0U%2BxQMOiIji%2BBsApKYN2NiC3stE236Hueh5CFFCA3GOAummO5MgvkBF4TUQYGLrGZg6lJ6GlLou4vsaktzynbDqpALTqM0cEIQSs2N%2B9YLx8zcYafgOd39lLEJuFweF9a2F8CZJV8pKwFyutkI6MjVMHtI%2Bj%2BmANYamPcUF0FoJf1hp9OCZMQjWO9G%2Flx6l4O7dmv6VLpFo64YQ%2BUSdO5vPSR5u2E5ucn%2BlacK6kMu2QHd3yefaVLR1SGjqpKfbzwyeOJfKh8Yd8jIJF1gKAq%2Bre1rGfvzDiV%2FpSQvm0ZSEPVG3xz0UalNuza%2BcpSALSB9bUzqhVgVNr6LONuk4NKOX8J0A0QQokSwLoIyfOfmd4iAAdX4vzkXnd2zSxDZVqNMr5m1jUJaIsrZc%2FmoZANwImi4b4W9kJzEWC8a8GoL0l9%2FVCJrBTenqFAYHStXRIUsqn68kyY3CnLWeJb0OVr78NbMM3onckGOqUBkup%2F9Gf6GM41dkEgqt12oHEimcJy7l2ndevyx03gN3vtZ1tt4Ef1uw2xRkXrkTNsTTnhYL2ZsMwPaJRGaGs10qBeK2Od904467p2%2B9z6V6xqzQZvAaHT5AKa7r6INs7xbyIaAHjG52r5KoVolqClezkrI57MCjVoloBt8b78mPhUEbka8I6%2BjNX9ke1dt5fEOVVoJYR72xdJ0JjrXKusagnK0SBj&X-Amz-Signature=0ed9b16f9018b0c9d00a571814544af4b8a57cfc019c7a8c467fce3b12c1d522&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

