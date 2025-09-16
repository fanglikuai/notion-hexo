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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466727NCKGJ%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T160043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIAFvFnyFpIwXUcoQzzSPs2vZCrT6y3VEkw%2BXW58PVDB9AiEAvpIlh17qaNtgJsJWGuI%2B%2FVUcDl%2FrEi2PaSmCIdqvGrQqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHkC8sMtFnJtZskChSrcA08OuxuNTt9m9SnB9he7ihajJFP6OHxfMB0bnE4%2F074dIgQmeePDqsjbzNsjgAWmXyEZk895F3Bllnf%2BKrv4DPi%2FAdchqy9RPqv3ng5D5jR3JTeqbY4qmh25sS4FU67fCfQ%2BEoKsG%2FaxMfhycy3i97vo7ID%2F2NCFLgcfzALXu5JAARTpE0QAy8XSxaE5xedBK1wE3wlNxIqlrIQsyH9OtCNQVGF4nKJP8adR0SYQZzYeoWzHeZ1uIGt%2B8OiMTffcpDTz1BUDxwlsoqQ264jx5zMWmLAlzCOsz6POR4vO8DhT6ZO5y%2FyarPkjO9wkawywCqjBbl7T2Ka%2BXihjAgh9kFsoyGZa5WeggENoQGFZ2y2uzpqVnHn3lYyqWYTqXpDtjOxd8hrj6j8rR2lP4GgDpMQ%2FsYLeCXd4bEEwz00ZSmzPKMhjx8E4PlxOBK%2FqaD4BKP2bVUKggrCK8ILiaLPwgDqxfRs%2FFu6l%2FnZS%2B81SJgmPHlP2atl5Sv5nhoSc9YE5GD1YZiSuR57wMhzIYcjgOzkTXIBd%2B3%2Fi0kgbxFPtXKfEYmgJmTMXBg1EKLBiqIJRtqXEupxJ%2BT6we9eCQcBX3%2B57uF8LDPtInIcH8XXW2jSMC%2B%2BvVujO5YWs6k68MPXppcYGOqUBHs6%2BHsjf8XT84OqnxuwI3Bx2Z0t7NyPet77pYzg%2FkwyObx14OB%2FqA8XHp1i2X3dZVWtH19T51RM9iCwzjOAaUDgHfTcw0IU25f6mhv%2FCuXQv55fopdsRQNcHOK8ivius5aY%2FLR7aHeX54pwpRoog9H%2BMb%2FhSHQcozkrE4ibBintr1zVDc8z2TYhCnQGUZhReVDx5GTumWg3HD4s%2B%2Bzx1TSALBpBM&X-Amz-Signature=7cce30f219efdec4d27f65242f60135ae11d49ee2bdfe2ebd81c5fad444bb1c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

