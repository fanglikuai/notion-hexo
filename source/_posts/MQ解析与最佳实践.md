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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SSUHLRY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD2AMWoPR%2Fx3r8T5ZDY8qTPnq0I7yZxtID2vpbOwCwWbQIhAIctPpLfVh8m02FtbZeKmvYYXXl35DoU19q0a%2FVTuRi%2BKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz9O5ZqZ9BFw9yVgNMq3AP6muGcLFOQwJ3sP47Xke2xzoA9hCvMITa35hcPzYzjftUYQCoQ1fKoWDmHnOBYaDtAWZeg25p%2Fzz%2B5Tew1c%2F92IW3AsFQlJTY3sZdGGSLhpASIvP6jLjhZJ5od1gwfCBBGLe94mVUIRs%2F18Z5bl4Nhc5YDATOC%2F7QQ5Pbx5TmcWPbk%2Blo5KhEyOi1gL9sLE%2BlwjswPdIozjUmhMQB3VUXBwpbDMa%2B23KP558MYKIYnp20o%2FXUUj3uICqeQdIVPVszfFYdxHz4CNaX%2BAkVABXNjTygvzieTM7QfDhTV2Ngza7%2BCw14Pgiib06Vj2tIgWe5OOQaKaRM3Km32JizPuo2phBDz1DO0emM7VWVMYui6CyxOSwznhhW7nqGPefgetnWW8a3ISmlcr%2BqeO5p0NqiAAJO8ao4SAvVXPC1JCg6%2BmDb2nGv4o0N59QeO2sL7KiqB37PswE08mmmKkfzgbl%2BPN7VQKQtjALD%2BK1%2F83YEf7YC26u3pBQRmtJhKOhEs7GLc4BHWuj6UKFizZB72blSf3JlomiQkWSRSc670%2BsQnDPcFxLlSpLC7R5Kk6kb2A5ZpawKbPemTPALoV4qv50DuQaeqsuffDQ%2Bive%2BaixTxE8FqR2SwcPtX3MAnOTD%2BqJ3JBjqkActYvwaWKcwCVYzl1AJFBs8S1gmCOjCyHyCB5BjAKR6EIH8n4gUu1GjZHmblhRoLdFW98IQUDm1x2PgmKqfMIY1OdmIuGaF6d4zNklBvgVPhpB3cr4wVkSg56hB55OABWwiTipcCsqf06jP8wdYDuGB7jcIgXAurzzs4GxWfGoMcQ4UomxZYRpM7xAgph2C9VotcdyuzedXKpVk%2BpZp5D5iuIZzs&X-Amz-Signature=14c7bbee2c408f47a06861120f8ef5ef72a455f54974f6d6b0754dbf34da317e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

