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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVE3HNC3%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDCjMaw5ZJ5VhR%2FMvNhrIcttAs%2BGBpbNxyJu0%2BklS8XxgIhAKrSSqxSgxMumK6T4BtFJr6CxqkoU%2FpYTu6e%2BeegU%2F9rKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxywfZ8hpVNhDFjnhwq3AMBcHPXxAaMPmLrUVqg8wbit4%2F5e7A%2BrEf2wMmD9CmnmpWQvjoZcj%2BzJyE%2FNG50vZWYrSY%2BRsb3wf9wNV8vWuPVTCcyjuQtnHhocKIopKkuWviDBxvUyB%2B3jYbmWfh5tw3spBQeuOV7H6uvzx5dvPN3WsAuq7pLCXqJEGJ0KHAXA5WpzeWd%2FuJkSL%2FWDZ5Amdfz3ClNgs98YXRfZRAdFkcWTDdw%2B7CWDf%2F7UxVNJ3J0UdW%2FHK6yMqyTohHNNtsWVEfEy0SkkNiRI6YmHbEurRkFcb2woVTGwShY5Rj3zXJJCCm6pBE8BIibzwfi3xECSrHbjxpK9V93AA9qMBlsEE2l1nWxcfDNbv5a1hezG7riabSAWTaxpnJB%2F2SoWtP%2FZlhdmCE90t0%2F8Wj2cr3CiPkyeSUV1PsZfNU1QLi9UVtXt4UOI%2BEfS5DffPSX9KNLneAM93OYa0s65sEzCvG8KHWA2zGHHQWyJXXKkAJRMnDuBn6mkliaS4%2B%2BPN2nzU99imYHzmibTAzofyM1Q2Qzk1ay0ZCbOl6cGMKWN%2FZ47XnSFOyGEswAzfCbN0l6MW9Y4My%2F3qZozzTslrFcmHtGTWKARkjzGcQL5zgjJYseSSCzOflW2IMoSPJJFpHVBzDl18rHBjqkAemafd4vNamiPhbDUVPEbIRGfASuTL98RYWqkUaVtCWJ%2F9UdVBt%2FBHkGZbvX6NdNt1WqgoawE8LBT45dw9r5WuUFI%2F1fwXJsb0Y%2FDOafhe1sIDkvHf%2BN6ZI7vrfeqKTzuFeac3Qn79WS51lpgBNYS4miVklGko8DrnF2mdDQzjJRJSH9vvIuJIXIp1fiqCbP%2BZozXvMM3WNbuu%2BPcUTZTD9QrVAZ&X-Amz-Signature=cac5e8e85f772cbe6a4e9313d9d6591e9c6bb61dd3fc19ab10e70780b99088c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

