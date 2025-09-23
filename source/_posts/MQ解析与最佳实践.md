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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636CYFHIR%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQlzIONxeeL6udL%2FQ%2F4q2cFLp8FnW4oG1I7hpb7vtKpQIhAPyq5Z9bUkcPHW1yr5hCieMaAv%2BiTwTF%2BOJ1irvyGsRbKv8DCEcQABoMNjM3NDIzMTgzODA1IgwayVUzxmy0PURuM6Eq3APWW%2FPbKuodvAKelLzQYSWIGCl%2BxfLWVYMwjH9omJSUQ00461OPfbAvxWXZBJQ6Sz4buy%2BCdshMfQJAFpCKtyGWXtIESPr2AIyq7tE%2B2eCPFixt202ARvOgjTnWr44PHPxjkXjWHXepiHTmRe4TGOwo0McMM5YFNi4wUHUsme8%2ByYGqUvk01idPZcEQXZ4MXQy6stFruJayGnMu8zdrDps25COSU1zYBNEhYo0ONe6M0t2kKBTfR73FfZzYesg7ttbSg2iyoqCaPbL39Q%2Bc%2B3athURA2ndElZEEn9Qo5c%2BJWU3hIV8kP7yrbpqZ%2B0NnMlkrzjKEwdJ5atrwrl06e45oE1TIukOG3XNqbmo3biJMD8zwK4Ud49BlXa%2B%2Fh7d%2FZTGaFDwLH%2BKkmFGsWYSSCf2rzQOqcQ4YGJ9amhQoRM1jN1P4PSIfABDvsPCJNtTpVm%2BQRHf%2FC5BReHG1SPNDIVB5yWp0vJdxj5luri4AGBrZKxhSgSMFJ2KknDR1afrwJoE70r7daVI%2BEpFWi%2B71rcRXoSVnb5Ue4XcZCqAGe04WLowAf6vKDxqrSF0uNws49vBKhVjxIwNw3TFDwkcd8GY7dvf7Feo2F8nXAvTZEgf0DbKY5XVwVqYcZX6OAzDD1srGBjqkAf1B%2FiqQ2Ct3CrDkoksGUDy6t5ZbqIersuoQyqazaxSgpipEg8fqqIZbjOH34loGP6SMX8cEQm1NPERprJv2cOJQqQDbR2Kv3nRf8JjlkMQENhW3%2FHuKMBzqUnvJ6SeJV2aAKf5RViBdQxNw6B0Aijah2HAuZaa8cKKbjFTOQzUwTLtL97gdhCKKZZtQGeDXUpAd4uDnSkSL5igjNtqEzI8QjuD0&X-Amz-Signature=49a1d772e832d01e5418dd5ebdba2c018563191da3801e3d5cde10341d953759&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

