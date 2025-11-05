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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQVM57FM%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbgk%2FIhtlkA47dRDkEkp18EUw9ogsvao4pdjcljMH9DAiEApBRG%2B%2FULP0KU1Ja%2B%2BTO92NYQF0uxVso6t4%2FPl1xb5MYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF45NhqdlUwUA3W04yrcA%2FEPovxBCn4y07Z0UIz6ivCOsbDuOuFjexiCU5pKywi7uKStfOa2MyUbQlfs0UErMweu0YCdZNYpUODk1XAffclPmd9iZ3Z%2FPQQZ0IOPgE11dW9PTGoxLyHZWvk48c1lwdj1e7DwhNGdI0%2B64L4ZU%2BguCTgKFfQNQWcEZBh8mDAoMmf66hfgV8Nxjis5ry8Lxzcv8Fcb%2Bg7S7ndcDwDeJ2qS0W8k6XkkNF2fWUcWJYsbGrJaEj9tK7w8Ci2e7yamKzVZDNHy06cfc3CWHD0Bqhx8NYtVhp48zg1zvOjPqNQA%2Bk9fpQwzdKKDNohNZWdemEFq8czVu8y0cqjYS%2BvJHoPLlT91S0%2BJu5GvdaaowHSaukfEXhMEqya4RrmUf%2FakCs%2BPM34hlXABhy5Ab9VCNisp1S7kb4HpwzzXC%2BNO%2B930ESyvKh7UVFyPTOYB5L7RrGExWtticeR0fhMiL1a4bO3pnvn4m3X%2FdYOZYwXnoM7iZoaX8eP4KFZIyzE63d4kG0vQXcG5KFnK3edrC6bHGm7DP35kZtB2pmATZcYSfkoa7pEfsWiTjo1UJZUcRp2F5r%2BTJITvKSjUnkg5h8QQoxhJmzUU3J97hDQWOou6%2FSwQd%2Ff9ec7w%2FWJn%2BawjMIeGrMgGOqUBCADOKsCn2OUvspJnP48%2BV%2BnCnHEBrSzymQ%2BCy4ufrCkJoHoJ4T2T5d%2BjG%2FUGzIpMGcmNnSd1u5Sotp0y5EwTEYV0a8h4Hx7iDRvBy1r2PLxYpNn9qxrQa%2BKcGpLJMefwjD72SQwsuwwL0SV4hSlT8VLsDFoXaYqLVfuAUVWKlZ%2FSu5lkBx0CYeNaOc%2FBFj2BPb7ynIdHvV4paElNYiVPyWa4MNFy&X-Amz-Signature=1b8df94b143d2586e02767689fc1aff57f482e6d2cbf13b50a752e7a981d1ff6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

