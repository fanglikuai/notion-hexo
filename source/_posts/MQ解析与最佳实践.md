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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPNAVZHZ%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T190052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQD%2FBbZLMXTJjbywmXOHG3r3ZG%2BQaa50d44IN0xJj7AcWAIhAIe91UAzzMra%2BxdjhDxZLdr3vu2RFwVIW72RaDGR5E6WKv8DCBsQABoMNjM3NDIzMTgzODA1Igx8zmUBwkkTRtGgILEq3APHesxTx%2FHRtSrx9HQmGEsRFDhdtik5aaDSp8wc8I5Ear6rNWO3HEQBobdmW4QCGOjBiki%2BYsJdYQYS0favB8A6yBOg4NAnyu8%2BPo3IJfuL5s%2FvEuZzZGcqB6TKI%2BkT%2B3pVLJTMQwt8Ja6geoCIO2eMSvxQyb4jp7VHb7N%2FJqB%2Fa7ES6t%2F%2FSh5Kbf%2BO7FZYT6fJbI49egSOfBp5hG14YqJuyv5XIDxuZVYuZspyqJ1JUPLEUQUB60AQNHg0GzokLbD7ZZ0UJOIwL7ZP08KIQEYocPB%2Fm5gj3cpAi7eC5gMesJJYy7%2FUmOX3mGOl8HJUVBDGkStc4T9eLFvPrADBlt%2FALoXgRDMZQ1YQWYuuZd1t%2B81WqLFpJkv%2BNio%2BogDjFkBZvq9vKPF8bb6PPaUMV8s2BUzN5JequpseITj55RLcl1g%2F6oQjuDuc3zOLuee72kwisG0jzr6SrBBSqfi3KhqEfm2cvtUsWidvj6cDDgLeURCGfeCRwg0kWv4NGf%2FG2mjV1Z3WAumc4eZ%2B03dT9sqzuFJcpufVGNITLsbVHYUTsNnSWkYEQvnykdBBthygUbstioJNUvTA4wZ2uGoF7iPRFP7n3S4WGUXxA1a7fsp%2FIyWBlsxJQmqVIShb2zDlk9%2FHBjqkAaIKRNJPDChlQfGOFZcLsy5bi4H%2FtGfKFDODF1jlI6yL4IJEYshivXIfolmogooEpvKgU5%2BZ2jxuwElfzAO%2FiYyeYMNThiT1r2IVOm8TCBYO1iuqooKUscsiDbSUdJkEcBzrLzMM7%2FY6mMyAw%2B1tWTWuTfmoFGeD6ZcuHjkZMgg%2Fg2HOMYcyCGLCjnEisOvIM1ZfK9xJTvxoe%2FNns0QgvZVN0PJB&X-Amz-Signature=692ace455fe454b244354548bdc72ef73904d334171010538f6dfd9ca802c07d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

