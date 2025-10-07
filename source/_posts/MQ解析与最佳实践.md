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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLJRR6DV%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD7pYjByHqyIan9ZkQHDLEvcLmfhjhMw5B8EbAbK%2B5wYgIhAIVZgd6ZmTW%2BX7ZWHGq%2Fc8ndfRw5HmwXmZnn952l1m7OKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXPPktjyoa%2B4%2FFBzMq3APO5Mg8fu4WCxMfdBuc61RHHhToi1%2BS8nT91tHVlLEkwQ9b1sa1Fr%2BtF3VOM4R3OqDAP4CDQD9NMLzaeVYsTIn7U5uEGafvMKUNJjx33%2B6qiwwrSmQQA0WE3U%2FKzPQW2O%2BuTSMC3VwjD8DDLBjy4ZTU15y0soWQ8LQ5wCn9XSvNfUHH9tR%2FCPn1aABTVVr%2FlzDWNVnrfzGkZHrUHrJ%2BvH0ZwmmRbBdXwITcL4g0Ngska%2FPoJkuJsTFZuVF9n1CaIiScYgMI8oFHTmNhKExBBMkFTyjkKQlWVyVBkTZ4b6iUq5P7ZjKPQMaUM2Ku%2B6AaoTAfvaBALn3GoP1%2Bf2KWAUE0B7NLc4zPsED1Mq7MyAYhZXk%2FkeqYQTxRZC%2BTyfuv%2Fv%2BT6dDe%2B9jFI6%2B1PS3lJJGq857AGpnOHLxCNrgvuszCOCLOT%2BAeI3bDGJPripQheP8E8tV5rdjBoEMmLVBhSclC56RfihA6MnKalN4qAO9PxoAvUEtOMnXiiZQajlsg9bQXsNPEGYel7cDqy048UzxvDbD4zrY6pK2tv3R79DVysp7x6CnqUbRmAajqpvHL8Tmk9itSu5V%2Fsd5Le08dpiL%2B1j4ewwZCsu7OXQ18CidTec1tXpqga0fY4Ku5CDCg%2FJPHBjqkAe9aTpzs8xl%2Fe68EVMpAF2sxNDISxylGtYlRBrpn2UDULC5Ekw5rXHLo5mKGvf5xYjIYd1iypdeASfBaOkzjNZwSXmzOq4yIQWzz5b7gci3Aj0JA6WKql3GFPK0gfpBMRgklOe69Uj%2Fl%2FvkezWJKXoR4LZkFb4Sr9v3789Oo9l5QSU1652wF3rzkpbdzNaJX%2FTFgbSWbyw1HgoLrcAkhaRWHc3o8&X-Amz-Signature=89544e3933f96d1b8d527f84567c059bb0697ee675d733a75e1e9bdc1e693a93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

