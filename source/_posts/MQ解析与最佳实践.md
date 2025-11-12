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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673G7U4TX%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDCOR665SU%2Bodwb2rfRAXtqX6VMMElkAVn8FNnxoFOjKAIhAOYSzJ1cskOcv3P%2FXTqLVgbfiH5dJ5shR74P6Q9o32oWKv8DCDMQABoMNjM3NDIzMTgzODA1Igzg2kLjh6j9tteR1nIq3APIplJwD%2FzdgP6POzlLKL%2FSRryqGuU2KzYm4Hd1Trag76pvjb2O7uA%2FgTpNauNbrS%2FK1riBiHwJxNKXp8PrP9zqfybsgg1mo28%2B2B5DoxWQP8adg81fdCIndb6vNh2p6FqcGTsKtLpV5oXyIkfaWJUBX8JnUjho6CBNSmW%2B7%2FHuisUWh4M2LyGoBE3PtbbOQ9WpLZNMKWMMJPYOS7q4g%2BoEAn8yYBT2Xn%2BsZk1v8ahrxNupHBkPAwuXQ%2FjkjYjc9v%2BBS5Kta86s4DIFGrhE%2Bo0bpSNP3RbgQez%2BkLsrVq6wzCeM1vDoDxGqYSmIBkha44ixUaUuBP19GVQmpzyEy0DYsS4KvJgejtkQoFQQ1B8ihsvmU%2FZHP3YwPspV9WmYGlXOF7TR9thwzu7buPSPhGK81QxZFDN8WiBGJAD13KEtBZZgYhGiPNJ6uDj93EDeU8dOv%2B1KSf6edLsQp002Xhl%2FcG0kmaMT5tMlRlsQhSkcQTS6VyV9NEMgNs1vnzilNU6gemoegQG1Y3ku3JjIud5Gp2fLZz5rhxztuvU0DAJsPx5EV1n60UNwLJ%2BOa2ItWGyk4Guol6NFLTQ535Q0hzPqXVZn0MqtfIB1D8GvyXSYsDBi9nNucwIXb76I7TC%2BsdHIBjqkAWyfhTdFapY9eOapyN4W%2Fzxr0nSy%2BKIgp7df%2B8PwWdnUnXipoipw3660YiXWbiB9RvUNlLvUVhOsPtI8Zo1FkAYktlB1M5fKUypGwQSj6wJlg9ePjbfCExDGPQEsxiCVXqAKDohe0FEsfK5kD9oEL21fWOZeoJscxMK71IAJfoNjnjgNQ2xfw9AbJXLDLrfsxqzFfcMOmfeHaQqF69Ylqj6RGUwC&X-Amz-Signature=ad929a20e9a3358d15604edca40091036cb98b442d1e833079e550b8d0093f95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

