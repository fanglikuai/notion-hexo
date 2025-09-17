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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CLSP523%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIHouFWs7umBK63iXbBqg%2B5KvlSkZGpbfkZ1MmSU9olJZAiB5I0JZsldDEzFfizhYkWr%2Fth%2F6gNwpSKIjiS0sjL9Y%2FSqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrVcCzYHlsNRILIzuKtwD07GN%2BukEjFY0toM%2BLoZ7xOyKSyV3nvbxaKky%2Be1gDfEs4ZEknitgDhtuA7tE%2F0rzNUP69ctUOQxfWl%2Bkib4qvdga66Cjk5rhGPo4Db3AYgufI3pMkzMwilvrRQUQuewJFe4DG%2B9LRQji23BYt%2FwoNh%2Bogwg61EYH0VkQ9XtgpO8n1oJUJom4%2F84mhflS3vzsdvgf24nhhOKmg%2F50FYxoDPfVPwlSFtsYTF%2FGy7COzB1BUjlpz%2FbPheWTv3wUFnJhWrtAMJXpOBGMzlPdyzr91bodcIbOEY%2F0LcFFh9w1ePFdwJJQSwMmBfxJULp1jOsKUiEllWq%2BRhPSifEwICi%2BbY7yC%2BrYrtWWDTmaXbhBThXvgXscQdJAVVLII0RL0bLpm6jALQli5NSjx2a9d1sY8AsAjU%2BJ6tbB1dA%2FOL7J0l6cGQX81WcRtyoSWkRqgKmJM422aDnrScqP%2BwmHF19a%2FuVFcdyucKqYdANVpDCnYUbb7cOQ5Ibwl8lpJZ4WVvCNfqg8CtsaMQe0cMrg%2F3%2FOJpvcFjN6AHe90DZaAvdGYF7y9lI1qFZGx5zGQwTbNH%2BhIxZjHwWP6ZeTVjKxLuhS6O5Hev39R%2BlV%2FaQYIrLXsMlbuSobV8FjLh7qd8kws9ynxgY6pgEWRBPQy9jUlZI1F2x4yyJD6xR%2FjdcJRFrZat1GUeZ7gr60aMP4NkQFdqKRQYXGpttzJJWkjkamvWF%2FyQD2q2u32hAQJgpLJKgt3wQDY1fyrcF7lQmykvDe%2F3U%2FeieKFvLO9h6JFgL9Wsd2YyoVH0VmOFPG0%2BeWOrrEQ7EDVUmmfAGKhxpgVTWGushPFg59l5yw3UvJe81RWFKy7oMDqMpW0nTa0I49&X-Amz-Signature=fd96bef69768c53d06b496a134fe0ca5f8a3f9296a7b9bff68683135a7ee446c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

