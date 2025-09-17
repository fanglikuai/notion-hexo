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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZ4XZDQB%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIBmjsgzq0TILy6yvDRRqZW4Th7FBpEQhW%2Bv%2BUWW3MfW6AiB8pV48kvKT6hW5OYFVYMwqaklW8PxXQ9licKeFOxMYlCqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBwJsHiIuFQ6EM8EcKtwDIHAScLL6kJzkT8fmKZtmOEPGS%2B7abNAk%2B1IkJxC16qCFudfvIbzMvgQrPyFDRN8WrkXr9bxFe2W9rElE8gsoEeInHaM1oO0cvpRQOemKtf9wzigc0RMiBqAiF6SvxB25Nuuvc3t%2B3DUIwmklt3BZGe48GaEV11R9%2Fg4P7dDEJQp%2Brt0NGwZDPpp8ihNNfhIW5tsbJ6jZQwfG9%2Bj88%2FO4BuCsH6lqVyq3UbE2Ebh%2B4A9P0Jl4iK%2BUIo2tkQm%2BF1H3wLxxTewcg1koUBYG8t%2F8W1mcJqfCu3L5RN2HvOciBpXOkTNBR%2BWpp%2FPW5vlonw5257ohLC68ldkrfmWIpMlCA%2FN23VxEDzye34t4A8VXV6Mxyc%2FQSnRXX9L4kWsbnRajHDZMy%2BuNpgwM4kFhUW9thvKWfmAQ7Sq6619tAaEO9P%2FkrYH6IpqfuMgO7Q5qGywdGWNOd6E1EKioLngBKFZefHNQ%2BiI%2BmMaa%2BdLjiPx4yvTrteNWw1F7MbIjzVeYNUjg3E%2BY14V1Nw38y6bjLYG3Rl%2B7eFZTp8Ke5mvNRNjTI15P7WDMPyd7271LwkjAuNmKLnjAtK%2Fis6WlxD8ZE1ebE17Te%2BzBvY2LZM%2FvHhNTGqiu%2Fcv0Ur7qk2TQV00w6%2BipxgY6pgFLO0JfSRoxd6HxaoAusMIy9aVo2%2BiBxjLohuxQwSLJPOxuyinQTioZZ4HNIbI7OhZtZ%2BUoZ%2FgPQ83asAL85EN2mrkABwmkocLNGTbyt%2FDUEQonE35HPwpHUeO5NGm95NEA5eOPYAR5foweneLUWJBqqzDWGlHQnQkA7Tc84LyIv%2BPnrxevTnYcMxCJaxSJorqI9VZ49U%2FBNJ4HdF1CFzDj11%2FWqL1q&X-Amz-Signature=a8ff658182811c32990a5f316acbd9f0673f03e59d3e86c80f23b96ca2b60ded&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

