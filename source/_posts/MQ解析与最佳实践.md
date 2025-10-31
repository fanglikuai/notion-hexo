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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMKHPWAU%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHb4%2FAetjurL2CTUgK%2BUnRBIKzSELMkpJYgbD58vV%2FuWAiEAlroMO6hBA8v99zzXBwKhCgaVqJMcKvluFK%2BPKPeK5DQq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDH7fmrK3hDeEM4rrUyrcA%2BfS3RFKwn0X8oxDjQ%2BNi%2BuAsYJFhI0AHHwvmpeyTGzOJG0lafy45u2Xj5jPQRg0Qds6R589Qc61RvskYXa7xy122jFta4bDQGTgkivFWfV2tAGE9WfGbpPvgUJF9EA6457IHQJUaQgONuhUFvBOwnwHwQBZGq3eakqnc7EK%2Fm4oYKAPDOuBMHtCKYvZTxYNgfpLszIkIiVFpRCadN0pD%2F9%2FQBVFfW2bQcLYvwWckYZUmS%2Be1a62PqMOE2gv5D13tIjAb6D3hhv7npiPoHNRs4uTSGgmhtYunylJ54NZxr7DBOjXFy8tlWyOzJ5a%2FTI3RVLIWkoT5v42LazcZbseIjprHfesB0WuE4%2BzS03NATxpzLiKQy%2F6T8GD%2Bbhbz3SWtHs5d1DG750n8%2B0V9p3e2EZmbPmDLxIG0GkX0qIZk9gc2zQ3HkPKWMLlvb5H3lZkbeYqXmQAqyD2MvMzkthbdIR8gDn0s%2Bj0B5E5Tt5JcpCVNZ4hHU0w%2F%2B3RyXTDTj%2F7UcBEQnQ%2FHevD9zhi%2FaNBS7zKUUzg0TE9S6Q%2FleTjmdLz01BR8gJrRh0RQi%2BJRSWbZ0R5kfwQVzo6KVqgrN2ZAfETo3lSAM%2BWvqLH2fWrLIw4pt6zrze18M8Rkq6jMOX%2Fk8gGOqUBuP0L9oJAlWBHBQh2UORewMstMBoW5ut4qYH97RJvrMBVjnijBdYrwh1HAQkMBMXOIe6YE0wKg8tm8zbc9ZvQjePyFpKIsm6D1M0Vhfd24rKlf2Gf09YccUSMIC5%2BUY6mAB5HROCtz1CTUmaYzE0xBY%2BvX6%2BJT%2Fc5mj3AGpWkeNpnBxwHgYNBVhpwlBTV%2FW3wyzLb0HR6gfVpHv3IrWFlIo%2FCak8I&X-Amz-Signature=9c0c5f97f5b455dd996c7863b2fa86ff3e9e75865d2acfafdf3510782ba72f5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

