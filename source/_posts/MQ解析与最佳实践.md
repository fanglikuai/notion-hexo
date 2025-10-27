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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DEJBU65%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiXcStMOZh3n%2FxAlC7K1bH3lQLsklVBRH42lIkjRyJlgIhAIO1JJkNGCwlPo4QZFP7vsvge3BpH0MgNST8UDUZfLw8KogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIYSVYeth1ejLwgCUq3AMAEHjlpXz6aYsTOR7m0Zx4IhJNW2VCK9e6fQ6HvzSOnt3cRQdOBF5iLlBlhf5PeBK68Q9Zilic%2F50tsqIyo1EqltJSJmnrRIy90Yj1JEscI1zgCNMX2dQorsOugdFdGzJudjckLTUl6cG8CqQfFBCfg8h6FV9O%2FON33Dc557LZroCahE9mo4T7LZivHBu8O8XAtRkiiXxocURVuxzFi5z2f5%2FDaUE%2F3ryQdDAhJ%2BqUfK4W%2BSzeTMGc60jM94cNJh14IkkqqDZAAYG4604VtJMAzfqCxl9dv2cf9l4N8F1MKCffm%2BiLAA8bU5OJyfJCekNEASr1lbZfXJyvr4hYEgS%2BiJHMj2Z9ZPH0bilxFhaXkwtGsyRSIb39fXNLj7zauMcoJIAqRRKnO4g7bpzL47OnIFCVVZWHMcWefvWh0ph0IyPJG3VEi5h3ASZ0njwrp4%2BjkR%2Bb9TLjYVuIZ0oZPpX8GYMXEYFtUN7MSdNadPQZYCk9%2BHYiNw%2Fy4Erw5ApK9NSpMwxALkr2SM9bzuOjECzoLegfAmBSoTip6ythGGHjB4HQHHYTHbhvWJF%2F0ptdhULNPjQ2%2BKHU3g9nCDORu4UFlKMdmncU59M%2FDuIj72YPCKSMM1p60J1o9kog9TC5kv3HBjqkAehZa8%2FPLFsWkrbQTfyIW30nyko%2Fl2T4Xh%2F9971qJgg9jVRxeroJtPW5JTH2cmHb%2Fih%2FRmmNUEb1jM8MPWB3PhZ3Gs%2BTxmiNfJtwH85hLg6k6lGN4J%2B9WGLszdM%2BaURnpui9FpsqAJymBAnf9%2FdSdixBBvpffhWCW8WWy6jeKR3Nm6KRmDdGkKCUZKkLD%2Fg2yNHykUtybOfaRpkiwoXzhJ25V2m2&X-Amz-Signature=5a3a0781fac4036576d5282dc8687b88164e02edbd5ca90f173dd7d6409da095&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

