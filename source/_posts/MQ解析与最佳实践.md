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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GU6KW5W%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFW1NVokSZdlK7ecr5F0%2Fp86mjxPgUN22JGf8LmIqh%2FaAiApem3RU57p6rSjRVgrxDfZdWrBCBA1eRiiYnHN%2B6%2FLsyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYqs3TRotHhvlf5jDKtwD8k2bwjjNNm6qY5GCOecw%2BvG%2B5ZXpJ2ROHgwAvS2SgE0li9sI0OYxXmYJJ7KXKRliKEKDoFojmFAJQTjP%2BUIf%2F%2FWnyFV8KCS6wym%2BT%2BYZYHgIGW01491ykK0frboRjUrh7LYWk3lwZy5WkB0ks0pyT%2BRRxcMdGpRiNNu4Tt1J%2FmYevOeD%2B92wmJqaxksAHcBbtSwIlhM3EbNnjh2iZ8iJL3ELDqZdUD8Z5kFZD%2BckqIZGgWT2%2FTf6VZc8HyCM0qFeLCUqK9wMkR397181%2F7VwPtLO5z5B53xHDWRDy%2B3BpHhcXIExfWRiv5vZcY3jcY9EoPLsleKsg86FjPc1fRm%2F557tFiBM%2FewCmeWFlt6BlD8Vm3NmFv9hG7O6wFE6emktSW6M3cldj%2BuXV%2B0tYCUhimkwEWuegkQActiqFvH%2B0uN8mlSIXwf1Jmcu4BwORzJAQbrevNIt%2BKNV42Y4z9cFuylYjf%2F8IZbbxX6Dmp4BNb7z9aL0A9%2F04UosgntFOJ4SKnqtU9LlR9SMPha%2FWxhEZqkSNMsO2uExnsvd2tXZkFpV7xZVVdJyZzZRMjlsiQUw%2FvA89VLobHrf%2FQmW5Zu6FD72POxX1aXDDT5Hg4bZdDrkRWHrtg8lT2qv1L8wprD8xwY6pgEgsFNpyLdE1%2BIsxMFa5iPpMs4JGBDjLCU4g9vSGeOMWP7sfWo0z3XeAPvHWQrDDNaO8MdIKj7lLUXHVjnypqzcPvYvLOGukBkb8AJrDs8uZsb%2BoTDGYplb%2Bco3LmVp9Ltw6n0Qq5nHUvjqDNr0AiwoD%2Bcf5o4Wf9BJ67wA6S3OjAM2cd6WbGDdz%2FR8K7YZ6Xk5pZs7PZg%2BrwXjWU1kcd2Vv%2BAvrzIW&X-Amz-Signature=9d20fe77f4c767c4f7095246c7e1609a9145d9bbca71239f6bf37ee0795a30b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

