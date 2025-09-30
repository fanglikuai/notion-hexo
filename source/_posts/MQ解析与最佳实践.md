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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDERM4RD%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCD8sSP9AmoeY2funDgBBQt2Rx1q73A3g%2FAdMnen5aLMQIgA%2BL3RuqmDQPBponygLVTdqLWr5WxXHurbFiCrHS7eWYqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJpjBZWK0SBDc3eCmyrcA90gTOfclXoznIiLTBmKxRA0QIzwEYmZtTNItQTPInRYJdeXcir4g0mKmNoM9Z%2ByrFLd7B7zVji1xDXuLXdpgWhguU52DBzCCToZJyUnJbQeIbSb%2F5rsfdGNWucCgpKJkCJIs8W3HK9t8bsUyqSQmjDWbq9eR%2F6r4oaYdOrL4KQMvtb%2B9AXTOihZegkdSS8wIb%2FSb6TkqpLo6ELG4asasbpzbybta%2FukKE%2FvuEQmYrUc7wKBUhGFJVJQQ80g%2BNhSfliVJdsdjt1zMvF8T%2FIycWPR3YXDTSLaYbc91%2BoMu2Gh0NnpCT3RyKW5w8UWJuamy5myl2c5zTv2KAviuH%2BGyhMlZWskdlh65usz40dA7mT1WQpHQcL4vzFycVQCap6le0hCMCpEnyOocW3TkbygYTA%2BtSHsAdp6%2FzS7M9Vt88A0bxjZ6syUCXFRpQR3FXVRzta7CmF5LnqLjWWzPIUOI4liKX2rkhrmci3xjlvpEfMcm16yptZWSW6vdXlEaoIZexqo2oHEjyFOII0y3kzQeeMdjlNuRVXl4dy4aKRv037LUeBquE8X2H4BNpPNh1VBIDadnzmhmj1rhWKSfLxBFHQ1PZzhz%2B5YmgY49MXOirkIor0FF%2FhbHla6nk1eMOKJ7cYGOqUBchdPy8HbAqVaFfarYQpCaOt5L8PrVo4yQz%2BDTc8ofsHFxg5j%2BLN3j6TrDb4uEagS87zYukKAq2VpM%2FN456J8ZpHnkTdBf2nHIWfYMhVOJDhL5BH%2FmJCSc3lDrGXTMe6tk8XAtMIMDN09ARA9MSWv%2F7xzvKMh2m4tyxr2Y8%2BcX7D%2FOvijwfnQP6%2FNdeTWrNYFkvgZr1VXqmzGY%2B%2FRTBCgrb%2BZrM5u&X-Amz-Signature=5fccad5f25b8a724acf4d1f833aa5a095491c9dcec1fc6cf12873aa8ba92b20a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

