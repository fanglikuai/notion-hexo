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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGSQNVPA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjxG6O%2BGuOoxaQy6rqN1VvbDjjkZu%2B6y2tityTQfaPrAiEA3NQdVgNEiUHOUsxRB62N7BVnAKCvkwnWVMp%2FTUXlYFIq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDOlMsv2V7rmYcWyQXSrcAzEycMsDsjeaWEqoiwOCYqfTefdoYMV%2BIjFoMqWSTjooTPWXm7l%2F%2FpQTCkv4d%2FrCt%2FpJ4dNcEsZtXAM%2BMEY4tIwniJsJugWZD8niXAwwDULd%2BJEwJ8SE0jrxGCSfEYGhWvMsa9ANFopW5WZ426zlicm9u3VWeVdlDFG7y3nCYckB8EoQA%2F6TmvdxFb2Y%2BKfdtOWj1%2BwmqZtz%2FiXi%2FEWRzIqUTErxUV9uhn0LKegyHov8C4moIIlyzuiIRiBL1GqKuf85qBdemYbxZaGhaHgsIFKxIeqNWAV7vBy1JedfhQGd9vWViCH%2FZRzNDPP8RvH8gs4MCwupS%2Fu8zO%2Bl5cs1veghgb%2FULlLqkwYZPkHmuKkiTOWu3DR%2BDaTM9WOTyzlocfkNaBg13mADonYgVa8gykx6XKHMGqcoRrQgZRluQ6B0fg9AqGNURPiO4lh0ksg41wMTl2zgH%2F1D1ovsMtbkvWGTOj5QbVPUVFk6kErI%2BQUfjBlF14xFmTWprIrklqIOJRiZkE4paQ8f4vgCRuQ8TKsjHItYqViZwIlu1NMc7sbdRqEolyZTf9pFEIzxph9q7CARlpg58I5zteq3s%2BAzkSrKwyTdtRfQi4Q9GnEOhVjVtl43zvYedTWYgK%2BZMPXI6scGOqUBz5CF1lcvgQmQaHCN5dcs%2FiR6cuEJa5hxkY3yLbYPL9vwfrZTpoZNcJunsyXaDAhK3TXUU4lmaXTLrDExJYmM5G3v7EdppxRZM4QmNOS%2FjwvY7TgeKp2gjaITgS9UsQLvMSi%2F6DiMz%2BNDizDwdgGlVdt9q4DWNFUi55YOvm8Gr1oV8kddmvEEYs%2F6uNBGnqWB9gZinldBE5mp%2FFiq%2BuvaJleq6lm9&X-Amz-Signature=8fa550ffc2b3b222fe9e5bc9ea589f2b2e4b1c05acd7d6ecc4391fc9ea0942b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

