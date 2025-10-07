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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635APQLNF%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIDYwVQZKcvNRcoMHrdWb2nRPZvKPd9DIwsEuivbeQ2RnAiBwGYgPpdKeD4vi4M7qtewgL%2Bg5sriKdhmY41vgoSiOFyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbZEE8mmAl7UfVxi2KtwDq1FPJnXAYybmfXLbwH2ovDcnZi8qnOfHhSTEznmquNDZ5qWWsUjl82vYARzBBDTJJBqXC6u8cApwBn%2BWQuGbNNqRxJLhbLj51a05SuW6h5APZ4gN%2BMWWnzTZGE0D%2BZja46Dq3E4FgJIReMhSeYSDhExUEMjXQXmNH7%2BqlVkJ9v19VjI7bK3ApLUmwWBEwYEYVgRtAdLLHbT%2B9XGL4mvEiv0KEEoi1VAyF2llZy4MmeLTyqLoSm89rqJ08whN0aEQrEJcC%2BlukHGD4rMKuj%2BelM0OrrWkRYuBtmhLLEsRZZgerDtApA5arxSBsIW4Ky9zxxATbWVIsMSbUJVTBRjiI2qRMjs9fpE1CeJVSsoO8T16hQGtm6mqhHEe9No%2BirG4aLWIdBtbC8RWjpa%2B6bE%2FLbp%2BphP%2BSu5kyrUZ8dOVKrhdyQyIftCVB90rkta0isggjI7gI61HUjMucJzKghd9oJUPN30MGF8Bdf693JPE6XWlLYyKW7w3mwDO7Y5a9N10xJTCG%2B5zvfH2drnX2XJJpAFlVFRJBIUnhk%2BZyV8iRAGaz2HsW1v5M1DvDOvfsTV9MqH0GuKedGWyHdc%2Fw2O%2F1KDj%2FDxvDq3nsDUKtiB9e59NCNjduSHUZzIEt08w7PCSxwY6pgGhm8OAdY5FljLIh%2BCEyC5R5bJnwylCkPkrWqjILYauq5j00PBgDXYJ8%2FJiEDrUVPggPnfzy2v1Syv0c%2BnkqDmfSSNE81fIGaYkoTfwk3Bq7p%2BabZNyZNO5WG2vLhaSYJAi5WZakgfqVkfN0J%2B%2BOnn7%2F7WSibSvvqvY497H1i3v%2B%2FwXX5sxak%2BZhU6aw4cQfMK0rPQtNk3L22o3nvNKiCkmPCBD8On2&X-Amz-Signature=d26e2cdb988a6e5b52dbf2e7343f5353caec8f0bb558fb337800d844abb3617e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

