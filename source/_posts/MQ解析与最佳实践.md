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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBP432YT%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T140054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGVOzIPiK1dKT5uKmxW%2FVLqt5K6S21uYrV6TK9LytExrAiEAmw41Q90ODh560GYPQBrymI9yueBkTjmyTWpXpyAViRUq%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDL%2F6sOaXllh%2FHpUSQyrcA%2BbsxEbzHryTyDIcfxy%2Fi%2Fu2J413Ka%2FAKgMCrqbwYg3X5uPZvTO2wiklwtTjjj5BAj74iSeiTa%2B5LLimbPBRS1LrV%2FgBYdIseWjmd7iRf2tkdYM8Ef5YlOoIZE6BnFBuE6PP3YpPIEGZlqDRpURSQrTiEfBi8X9vHcFj9jNW2j6VN3GhVQ6nKzFezwi2DwKx83D%2FdbqN85lJ0GFjyCJrX8gqG3uBZUcGJoVdKej7cidQEcuvR0LBNOcZh3tNnL3z6rHhzKrLfCURrAtDqM72wFDrwaEFn4YizdCgNjiE0D4x%2F81cwCsHMjsJdTQ0xsAgCsjD5CXzwvFyiYbZdMAPogRMV4JLqPWzllrafJAT%2B0G6NAATGCkL56vSPjxV6T7NnBBLfeV0gGvTIaGTQwaI1ZUTcoL%2Bvr5C8XodzsGkP7FgHr7tifiD5XiO8DkItt6m5GkiblBb1oIZF80cnjMFiYehKGXJkt015aucmMBlFZTeut13pGAGXd93kirAVm9uog8O9pVCzBFtjdyOM98C46XZmu2EyUOZ6OFvE%2B0MJxQBEp0DFtZt4bEW5%2FR6POsUNv%2BtMtqkr%2Buo%2F0DecBYGpiOrnrWASzWR8l9NdllTcqkdCmeQK8%2FhfgzHXPZsMK%2BK1cYGOqUBWuPJx8n5U9gjSP93f7N5e5Zwhq3rFzZTk1gsLGwx7%2BH5lDnJlYePwcgS%2Ft26oW1TWYv%2FAVdH9FGw2E4e1EBSzyFmM7jYXb90KH1R7bMoGnLZBkKlLs2qXKquJ%2ButHe61PXnUYSQUatJBiGcTXOJYoZLE0YAVTsyQRhHNuzwpL6uNwwvFbZ%2BuG0xqYOYtTPO0mD6etzKg0xV9P%2B0rKl9tbknWb%2BOo&X-Amz-Signature=b792551b112e0ab422c7711c0553e51516fbec54d6360f9f2020baa54ce1a7d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

