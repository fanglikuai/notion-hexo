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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2S3YC4W%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDclg0FEqML9QUziuMgcvmySg5lBO0%2BLY246ktoxFHgKgIhAN7joKwQs%2B333vBjUlNxcAO5LtlM%2FWJ8F%2FUmXMK2kCckKogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igydo5E8Iv0cKJpuVaUq3APwKhLIMc88lSr1toO45GBHnxaKg%2F6nPNXj4upna%2B6RmPK%2BdexegnK4Xt22oHmVCIAyLcqEpMeLGpTKTunrsMFezK9oLDu3cYkyCOI0ot8XuTOcqfLO3cqg4WmOXq40ukQB6jTBa79i1fwoYLPDT%2BWQFfy4S4bknjllqUEU6LGrx9I%2FAL17ZN2300Tgjx8QRAHfLaZhz9Legx8azyzq64nJVj6IOIcHqsNzjMd1aw7dZxjpDZFmkx4QJ1k1M0UmZsdU6cVyUDrpRgR2lF0j5NqWZXKhxGfUJf%2Fs7RWaFquQ0wsz0Hm97ZAPc6OYXOhATYq%2F5mvsgludLn7RpQHyQIYJbgagy6IJhlxmRBtkVXOdwy%2FvNQYalS1wWC6PJGqibo9xoTTZIZVBc91YuF7ZTVOZxyaIyheECU7Qdh9lsZlikUo9BI0xm76E6O7kChLB4RN%2Fgs%2FdaNAT6Hp3z8c90X0oDrSgi4Scw3BquqyK1Jo74T0yvFEfTpABGTFas1w5kSeXzpE2XdwpcILjEH0lijnzLC0W8AdQByhCyoNzlih0USZTYcM9%2B%2Bl7ZtUYtDt6SWYqd79Kvs8fVBywZLbQTWzRLzBDkFTNdLsbdvVrLpOfk5XHLokiWrmLczEj8zCAgvfHBjqkAV%2Fi5xbQS1m5u95QQITs9MSOnIjqYKedTX0q5l5YW7xdRw%2Ffi2sAxWf0pBXl7MpT56yzqz9rj3axQSYLAXFoM9XWcHCtzkH6N1Nh7rsOnSFYTOKwaOuNlCMWbIrzeUGwY4QFSDaUbw1xRdIUQ3RlVONi1PhNz7QSSZRoRncMYrd7vR0HBpIUcN4ocVkIE9w9b9KmAXpJCzv87d4m118CfMU9JZ8l&X-Amz-Signature=7bea485866ca8d3c9b5aa74573abe41541ecfcf2a0ab725d771d2cea601a1c63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

