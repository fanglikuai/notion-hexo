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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IQ5BJZ3%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXbtsJDuLQwiy2%2FRaKDmGap4L0yMDm2%2FJAz1BLR4ISygIgDd%2FQi%2FQ%2BVhFmifNR1h%2FyagK2W7z6%2Fuh5VoQjgOIGobwqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBy5myaPYGqLs%2BIF7ircA4roR54tHvo2DB64uaubpR7%2B9zIt3v8kf91Ls%2BLA7e68%2B06DCKzynhcF7D5zhPLvunplNekGVt%2FhnuL2ml80C6nqUZy2subRr5mSdfzPtfp%2BZMZkbj%2BdkWzqF8gY1zIz7kfBvtzUDwSXQcYvsNaqifEVJa1zQghEvtSAsPkSVzsNB%2BPZ71c8G9C%2FWXQRSzSG4Rv9vXdln7yfDBooPMu7fVd8ng10VVj1OdZj1QoA4qD38wWmk1ez%2BZTUgRbbssUMwF3yNfxLAajydCWjbfOSi12Z0DcllMeJAI6jQu%2BHmp5sPlxH8JePtvCiELK1U7CZFOBbPC12Kgk%2BhArILF0P%2FX16ntHNwVUQ66IueB8nWC%2Bo%2F0w5BwTKgvOWTgO1u89UsYyGmm1wnnNOFCbxRdB%2FQIRJaFSmRHDtYHKQSWnPgCcK7TayLs82rKgtiYI8vIh8q3LzrKnG48g8ravVB03yFs8VoVdF9KEdkOrXAJ9WM%2BLzMrdGszbA%2ByXH4Z3Ms8I53b%2Bq8BiRxE0q6DDTwOhXHQFFcSpud3ESc6AtLzm7qLkBCMK%2FKym%2BEdDF0pj%2FJSzvdATxHDdSBG%2FTNeheyqiwlT%2BR57wjS8DdR5yd1eH0AAATvL01uINZd9NflwjFMPmvjscGOqUBmCz9yI4wXmWx%2FfaKRdfxR%2B4d2V16yMtywA5v8TVdyMeY%2BbHtcw8EyqG7gA9Z5eWVO5ktePjld7TJEVGGMv4dOPHaSECw3fgCUZT6ef85nF4Htbk%2FohrIXwkN8%2FLTqoCufmXSjKcLtXUyONPO95ID8a4s1zpndwdASV3FYyZsw8q3L%2BsQwauKXWvhSJtYv1SDRWgv0qVWTrXe8pS6B6L5NiedQDoc&X-Amz-Signature=abbce8208577d51a4e43d2cee357e096b12f7735edfeda4e4cbdafa98bd4e92a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

