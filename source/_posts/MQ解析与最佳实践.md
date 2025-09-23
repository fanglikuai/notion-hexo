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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GYVDTO7%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDJPX2nlIfpx5pyb28ydm7y7ek00M3ldaQuKqdmuaLAfgIgAng%2BCPs5HDpATGdPGUn2hPpXKdsN3sJ8fPOVUweWKwsq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDMddsRw1FJFIqPwtzyrcA2x1ep%2BxxjaNQr04Gr9KtxaRJOHn2MNTlhBsleP7i0NDsLBK0GsI%2F59aAPLhV4oHd4e7DFc1DRqpVp%2BYouP6V5bDMjEcPEZzi6jjXBf8zj%2F7XMGwsV4i7azMIUKu6vuz1embjrh%2BXOhEjqOkV2Vz%2BvtWodsqGI7D%2B3OWk9%2BHQE3%2FcFmnJqH86pnUEMgM0pNK6BSEpJAN4nmaOm6XpTHlskrTXOcskatYcrSi3MSjZgYO95HmsOR7wWHkLJabS7a%2FUTAb2WDQSVUINS%2BGGA1h%2FfFilDSoi73FRmu1whxg5MR8LHqWYZRYUdvBTMb1tS0tkCdEa7xzoM9N8qGVKu3IIgK6EKnPmkLQ%2FuIHbzxwod1zSyQGSwJvqZaNK3%2FegJ5yk1fNBr8NETv5kqZmtu2a47Z5nH9Ng1d3OvPBO4KgfO0caNBBLTBL1UVbjqlGYJQBo6nvVHwaMaTIm%2B%2Fl7geTuZddfUY%2BLx%2BQfPRjPopT%2Fo7AOSk%2F%2F9TiwG1qXyW5NXTmbGMP%2Bw2N1Fs798qU%2FdtgNyG0DWfEZWObh0VjnZ%2FiLe6w29gElCMMGcAYiSRTmgrKmPIhRfZ%2Fzfwk9bkLI9X4zSqgQfL9BIGwI4tmBqX6K6XVoGy%2F0Vg0WaiTKTiIMOPzx8YGOqUBo0b%2F8xG0kMspoVtti03cC0mVlNVvEVrH9H%2B1heYuctVqUCo0NdnoieV18Wb3OyJQrCA8LkSf3ZB%2FdBHb7tgKAYX8Z7QzkiV9o1%2B20TaLIXDI%2FCb%2FUHdztMD1Dl88TODer4glkwR%2BBEAGbLbWYzlVjFKn02wfGIO255Ig8CHIqQIKkmA8LmaOWRdH4GyZ7sbkQ1xd9QZ3xw2LkYVcqX3Ad%2FB7Lmdx&X-Amz-Signature=b6a6bef8306f82b76ae45649a3e864beb3b528303759f137403a4e021e216018&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

