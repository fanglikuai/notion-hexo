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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLSOJ5QG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJIMEYCIQDEy1BXxEiRg71LFmLADmUFIYA5Om0HeA%2B7lbRcE4COdgIhANrDpVtFb%2Fe41NUPexe7EMHBDeapn2KGOjebRHmorvEzKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxsvrrtHTPyvjBh8%2BAq3AOnJvuMGcU8vNygiOQ%2F0CszJlYn7po3SeZg9qPdw1bFWAcQeOIa%2FFwzAWThSr2i%2FlXjPMAooX1FR5KZcaR2LANOetBStTp6nT3FD0mEwvO0Z7KvkFhZz%2BWFW5mRvMsB8xnsI%2BesVlqfvXH08THR%2BhG60fel28W%2FfKz8rGaixKHkYRfFgWbMLjdcrnnGE7a2%2F8cNkZtUjMCEFn30mLD%2FzonZSPJO4mAMQJ80uY9FxAMLD5xP%2FljNFHX8NiSTpAceheATqO94KzYhugCr0hIqwwtDk8sJMs%2BCY0SGqSYUZhLKmjyqYylKhwvWdePks82OuWr0gI4jMCH86fzakUkWXXRdVKUxqLA%2FRk%2BGLp%2BXUQMq9cERWj2Tk%2BY3TiDYALGWMjQNfWuwhIhcj0t8mAnPBZ5K54Ir9fkKksD3Ijbe4jblDWE62HxykfwfcXeSBYcZ4yhBlSTyVGfuPSzgefFCI8QTxZA5WzgCeD%2FWLMHf4y1pLFMkK8ZNQn4QojFeKFNJKxdeuY7m%2FkCYgGuqFy44vW3iy1Q58bq1f7ywTBVafgyckRjm6OSusS1LOpFUSjfx32731eEqisVFEIJ3hlgerV0n9eHyT93YQY8PPuyPoLiVohLW6oF72qHXv%2FOA2DDM8ZLHBjqkAekRJ1fEfSOWc1q8RQC2bcQMLd186C74LZvyrk3t60FRa0ahy0J9ag1aAVs3Knjy4swOxRoFMGZ8FLThtWCtpsVj8Ty5Ug8icUZ9OZMkXXl%2B%2F2Ksv8EpwTs7%2FxBsBxbLfno4ADKKUgw5eTF4ViJPFgsZFh80wRz89X179aREVqMbc8u2ncuDL%2F2drN61iHD9ePkn5C7y9NbgzXYicW3leiQom9je&X-Amz-Signature=0db145e72f54c73dee524c877b5e93b430dcf49ff9ee2ed6f4a563f7d77d0da1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

