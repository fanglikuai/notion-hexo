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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7MMYW76%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T070044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJHMEUCIQDL6nc70aiP5TSbA9FeuFSn8PijkRX0VpA56DdwkKScnAIgNMO0KonGjMZZ8CZ8A937MCCoj2xDumEp9iWxH8pPC%2Boq%2FwMIEBAAGgw2Mzc0MjMxODM4MDUiDIoRyU90gtDY8sYwCircA1%2BaokROhwW72RT7%2BgzetV4whTJsFCmLc3mgt9iodrHbRf4jsUkf7QjPKTJBk0AXKIW5nZnR2hm228DZgvpRIcQlFQ6RmWwaY7bX%2F9fp%2Frf2N5Bp%2B%2BYWtBDNWawJ0RX31WzrcZxhAdjz9yBgq%2FNqDiOZi0Eh6f%2BaErdjMPUWziYTvfBDRO6HGOebHmtRstxOVZFMt4x2IQZ2heD5MQxTuS7fqL0TSSb%2FsJjBfnl5hkToyg4d3UUy%2F6urfUQ9W8aMF0wE7PxuZ9KMsmW3aLlciWsiPquS7%2FmrQzy%2B3WDjFBkukz%2FbAdVTC72JAUgIS0nwcJI8jj%2FmUTsFBek6GxsOQ%2BOziX0gMySZdR%2BNyaQ9cj%2FVc%2F8jbYVzH6SDwYE9pR0DbhFskgX%2FtDWv7Jzu0u8D2dOX74Tsu2ghHGoyTVKmmmfEMoG3A6YisYLRCyxBwGXhzHT7ZRlkcUnPXmFB5lLPmKahNwZfoM79Ye%2F3uZ5tRnBAheRsMw8hbhmIq4QAW09Q%2BgdmleddLjRplS3LaiakfQp8Y3xi2KJ%2FWAOcB%2B4rf7vMpmZgZAAFE2hgi6yfkUpOafkXGfnd1xDZN%2B85rDagKlmWoEMCKLmKV1fiF0Ok4me6dYNidBxTbiVkogTbMLWrkcgGOqUBm2K6Pjg0GAC8EvR4FZXEmw3imzb6Eg5lB2tREZzs%2FGqbKgiqI7aUkwS%2F6jKydP8R8qWSQgrqbxdqBcGWhH62o0aBqUM5a6aNVzFRfnzYcbCsK%2BjgV8clbrvYrXzCePZZ7a3NPbDHnIBKGvknGwg7XoXJT3CkqmE%2BDE4nFuISyvpLEE%2FrMK1xq1PvdTnOqFW5jyUAlPlAyCJpqRITrlU9NfQv64KL&X-Amz-Signature=abf77fe5b5479f42e60f729fa68e8b74ff857dfaee0a305b4b761bf40b85591d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

