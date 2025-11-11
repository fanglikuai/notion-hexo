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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDHL7R33%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIHuagCeGVPJmkqJ92hEnHVli3LVGAjiKlyCAxnsRK3OYAiBVSX7l%2B9KkmRiSfaDlWcr8g0DC5zEWxwYkpwWSlOHH5Sr%2FAwggEAAaDDYzNzQyMzE4MzgwNSIMBxELaOmc1HkMjA1VKtwDJRh6RT2fRRBBF0sHBUEE1RdZly66aHwm8KLU2gOMNsmYOzlG1zepbNCHq7qTE5h5ACfxUvCBs2%2FLJ%2BE7gluOfkI4lmnVtfFfEGi9pb73k8SUHrnQZGvJGEKhUIoH8vTbVx3awr4Xnd%2F81m9sQ1OXhfmjdUo2D38ximW5xboYCECO3tDCP2hmyl0UM9mx3wmSS8uqdxpnaQ6KvI077iRIvaN1WVxMNB9sh4W7IUF1O%2BeSt8jRUz7zBKbhTMrxanhwuCXgXQkbkcKmc06V9Y3HNhdOFSz8YVgRtf7QvfM5Vlp0SOxj5R8xjKo2ap3oz3Is2VMD%2B8CFYX7cj%2FLls1EXflKA0S%2FIgztuefKeYmSRQu5idwQgszmJm%2Br8Dpbw6G2wnx7w6lQA53B0RZnlLPadwevfs3%2B0vF%2BO2%2BmzDjb%2BFP12T%2Fy829zfyiQLznR0pZ%2FQ43PeSawhkkvd8qp4FG3sJIUkVbEe2mR3qVW%2FSlpW0IaueCuFUo2vxISies4%2FSO750js854X%2BujJCCuNVm3yyRYNcCIXRiHJHNF032KY49aieEVKkis3K5JIw5p69SEK%2BqfRWLt8A%2BHxD1Siv0sxIqF%2BQlvE8jMog7s9ov1uW6RcaXcgmknyMNM%2BUmmkw16jNyAY6pgF68rHCAjAWpUjnn8z6YVx4a%2FGVhOuwS9jQlh3ws%2Bcmy6MdXBrwD9tPYv6f%2F3%2BtIiCB1wmvEzNQQVgMJEz%2Bd7t9g%2BsH1yI0Cf7kddBrDrMcxC17WIgx8mM7rwqw2aE8o46pM%2Fv%2BxxVN6tdcJZQ9fHFfJkSIM9Eqb7VkJ9VazdMyV6Q9KJDbJOBqPWIUO027fDn4BE0hFf4zqibS5vSJS4ZGe5z6zZO5&X-Amz-Signature=eb929eb9089a9b21a513981d6d3ff507fb4ec48ae061f7666723f890b9cad7f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

