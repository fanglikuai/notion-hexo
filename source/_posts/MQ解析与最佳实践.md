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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VSQVCEJS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG0V%2FGsqPD8MpZEd6lV7LLxsg5eUQyFalQb7Fx6ooMbjAiEAi24X9RjwZDA%2F98P0zdL1sH5ySdHoSsZVzwKLOl%2FQhvkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEnubqTZ2K7ywRIXBircA0atExx2rVlfna8QmzfJyREv7V%2BV7J0FzTK%2FyQOuLGuPFfA5eXyrlS5XTaYKo%2FzURfnZY86AIOKzPZUwVh8ExhtwRSx1YyYxNU4dndzQOy1FfU%2FrmL%2BRK43VZoxtFIb0uCc67bTp0v4%2BitLRfKj3mUCSdMX0NUlqroRwxTWP3VqMx%2FsbIaPbnn9Qcvr3djyXkXH3WOeNwHk74HCMuXB6MF1MpFQGK9O9c3blX9v8FFTEZVSGGRgLSwIPGFxGGJQ9hFNjGcwK01KaZY4wVsqyePvDBUVy%2B52D18%2FSzBsljPk57CZij3z4nkG84viS9QvmUIw9p4bGDRhJCmjYjWyMlBF5QUIYkHFQk6GCVvD%2B2sSV%2Fcdfx04s%2B8HdRC9iR0Ya%2FcoZQo2LLcNWKldrBgvsY8PZuZNCcJap8olsz6yPVIAVlwRgfj14bMsn9Bd9VMPBzznhSF96vSyqY04%2BVyy37bsPlwSNVnqia2315fi%2Bak849o66bLY4oYqekePJ8C9J7N6kEVQOeBVlPIr6y5PBtenURM4bNmwEy%2FRTLDWga%2BhJuvRKFf5RoeBRCxa5DtflE3aBCV0VjA0MND%2F7CRKepX3SxCImG%2FKje0DU8C9gdthJvC9tD9eW0Kqq3qlQMIbfxsYGOqUBbBI4J6RWcsYj%2F18TKKQ3MktO78xohInvDCMXjTsSwNeYMnwEJaPCF5oOnvOQy8pHD4kFfUyDJC%2BuhqIyWmeU38BKxOVmdhR8CIWIcEf%2FACAG5nku3rayT4gWqeHmSd11BF%2Fll4KlElk6o6TDhefHmIWAmoacOF%2FCEaYADGt4LgiHu0%2B%2FgfzwDJclK277X6CYpGl4QnVcfpmtnMFy4l5d%2B2u%2Bm7D%2B&X-Amz-Signature=5ddac45ba6bebd23ecb4aba36acfedeac9f68152970ddee6fec2d8518b150fa1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

