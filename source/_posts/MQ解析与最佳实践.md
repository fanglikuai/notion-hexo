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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCR7GQMH%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG%2BdbWEAF8HQoZo3%2B2dOJFPj2L5MfY%2Fzsi7pxKv8EvMMAiEArdYZFhUN0ZRjQpKUuXR6%2BsJ3Oh1HtfBEwsddVj7SzxoqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJiQbd28TClTd83bCrcA57bVBOp2%2FvUAmzpBc4U4Gqr8rjItTayqYLKwDcDViZMZbbOdnBjc4lllDXrZE0Bm8Kh2cOzZlP3vNJbc0pE5%2BMGjWSzwMgomuJ8LYbxCrodDrYK6nPzzbzltGyMEXsi%2BAyIsDcq9Z2dz9ksYxFogU1e%2Fa6Dr7JmFLqgCNCFOG7KMr%2BpmV2gaO9WOLI%2Bkr1guNGN6O6FDvjjFbLpSCWWhYiaHlZTDPx9Qn4mYQK2E1WRnRmXeckQajFx6XwYlgNT%2BdHyiEEXjvYybj7JCyRxC47vn%2FCUdSJP0lTSNNfQXCRxLd0R6wOYb0qeJ31jILXZSYYDFi8y3hn5SFhmurBS5ioygCUMO46BEvHGv3tvkpkSkjOipGcqlNu6lqdTfuI%2Ffp%2FJ52zmk64hxAFRkxlWNmMVzyHPdWkEVf4jeKtGU%2FBCB4Uw%2FiEakl49t6AgDVaTm%2FBKhQ%2FTAVW3VoTjd9p5XdoT1GtxGVRH0QT7uAGQRhRbhH8x9URcdqiQTTSgWdOqZnEbkXO2NaCT7qflUSreKL%2FGaZq0KY6TdkbB1c%2FuYo6eu5qM4NYzWP9xiguugdBTtf2dSw1hej5r0coAO8hBukDKMpfmUoL8zlSL99AMTDBpak3LyYVq8c%2B3B7pQMNC5sMgGOqUBhNSgJFLeZWlxg65m%2BgTjUwp6QL4Jp7F%2FgDBPXIlIrw7Hy7scWFax%2B6dFBkDO8IliMK7tWYlHGq95wFCK%2BNKZbekFfv6sBZpn5orePJ5AYVvEzZul93tIomEcR2Brz3nF7C5uzm3TLsESqoUICkWSRDLMM8bU40Fn5S7pVmleDXma0vLh8pUXm2DyeFgrx%2FtswzSkVwraWKuBJT5oU4qdXaSsR3sI&X-Amz-Signature=59bfa4c6574e41e5ff352eff7696817b2f372f71668e520f83c39f6a48704085&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

