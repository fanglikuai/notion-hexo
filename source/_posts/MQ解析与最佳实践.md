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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THKWRGYA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIHkhI6CSXOxc0e2coUNbwOvLIxNDTYvjyoY6CknLXqURAiADYlH9IZTDw3eHM%2FkPbVI4B0AFGmqKU6AFc6ECabN7KSqIBAjd%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp%2FAwN5k5p53qeZgEKtwDsFr168epwgipKpQivQknWAsTdt0kJ99rI7DamLYiwInmwH5%2F3x1vsBqena4Nl0gb6km%2FX2jHXCQphLe8YH4bniFfO4sQbiHEW7dpuWFmDdFQZsHydDx%2BdfayNsT0w5QxRsMHL55OpgbVx385JotSBF548Y3Ciglz0qEayUNnP8ImBmAGXrKobdQbwst%2FnFMGR0yqxFI0ojBYIStiWYiNHCf9HEq5Pev07jeUHWez9nqqcbnToPAkupYK93VWwhfIaraKvXQ5xVUuMcTddP9fYBXRj87MFVHcogEWRm9pl4pkaPwAqVaIn8YFymTvs3r%2FF9AFtOC2IqcId%2F5xjsWHknCBW1YQ%2B5XpP1FNGNEO%2BWdthbTf6AJvyfDQdSS1Hp0EakaW1jklOn0I76C%2BgaewgYF1AH5rfNCHLunWdDRzuFpb7N2gMo4FqmWSf0ngcCgGLS0tvRhqCRGtatqvYIt26UQIxtEGbF38nYCL9LVZHPp4%2FvZ3rkA4N3SX8grnL%2FrrMAK7Ouoh2AacOJJtClX9U03pmUU2nfz8C0qctA54zPaQr9%2BL99ohSUr7Bm2Jewqyll1MCM3eU8b5IpSXncl9lrJFcBKkjquXtuMt1R%2Bai6LynruuS9s0lur8U%2F0widP2yAY6pgF1vSCvbcS1v1bLYDo0cDAVQXi%2Ff0p%2BDl2JaKoOWU2IEGxQ76vCCVI9IOWOPP28EFDQmnI34T2dkLbq%2FMP0XT3JRKSYrKzSpqjAEJKNHhV3DTr31xBqtTDtEsKTXDbN80XZ2i0tvSBsuAej2OEUCmgTetO1zqHjvCI7JVSM%2FgcpMUZ%2FMxxVEf316e3jOm1MmpuMFqD9HZL5lhD56tbuWncOliAPAaFW&X-Amz-Signature=9005d8e265b5ccb0c29af665433d1a1f85ecaa4bdca156aa8a8e3353c0be4d9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

