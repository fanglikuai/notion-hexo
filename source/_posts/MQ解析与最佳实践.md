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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZEGTLEO5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQC%2F7G1YUS6lrhEGivs0KHxAwIN9n4tzuOqxQ44Iec%2FnVQIgeV1s9GU9UV%2FDtclH0z5wbHFL28mqKBMc0uqwaOuEbgoqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOcnihv2fjTM3m13AircAzqlGfMHvlmWWD88nYVhiOQz8e%2FluewrxkVidyaJGkS4HpUQOm7Qspml3s%2FOqyTIR%2BuQjfT75NVs951GGGFDSTXa9ZbeNrH8vO7m5Fh3iMtkbvP%2B31AVVIWnKOyhwtuOWyUwXN5g04mYyPCoETc9o%2BoYVG2sDmDqlXxcjptQPBRcGOySx%2FlZDGbW0aXh0Qe0Vq2XouZwqmNQcAumncslXii5AAkJ5a9oibi329vpVB7eZ2cfcJ7iLIZZFMiqtxiOvtQ5zT4WqyOwtCHgNpNpaHOCXKfKpc25xpHzyHgfiizsL9bkbtcq6Y%2Bc0Fvy%2BYztfEEv9lUQErUfa1ZRLiuSkDQPi%2BXW7VqcgcmGDv2nQ%2BbD98eeos7U5qjCyiZWQ%2FBwwxHS%2BVnVY3vFjqCSr0F%2FMedGm5mdRsz7%2B5AauZvGqYshDT4jU%2FrYe5cUs5UxO4dPVNLrcEs694UNbYlENoB%2F7KZ0nVmr0XkxBAhGN8LlnQ1bahFev1J%2F%2FH8uuIqWyoB6OwY66%2Fz6ZsZUBRrA92orREuTvrnio%2F8WboO15ahbTmQlm8NvgSjaE8WXZPzZmg04A8HOAM8LNvmwhShoiDCCkZcFJ2dOLku%2Bc0vHzbXfiTNBsE%2Fa3eohcjDsnlZ6MNXFvsgGOqUBcLEZMspVsT9rVnoublgbmCjZhvnOtoZ4XcR3ZlMw2BVniKEUm2%2BkO%2BUouZR3KGmRzmO749Q4Ff1Lt0SLS0L7lrVsIQjs6BVLq7iySLRudBCzH%2BLijB3j2E7GiR1QtjHDDKjwNPWUJhL2yLvk0X7rd4Sr5W7dEmPJ%2Bqxq2iMBeO%2BXCS%2F21dzYJ3a8CRyhUcp2o17pdf22th9hCv4k4gsDOO6zvz8V&X-Amz-Signature=55b50bec3d34a3ff1c83fed24826fcf36814b71bb1a6968141255626047790d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

