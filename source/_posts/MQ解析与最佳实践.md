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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSYGZRFO%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCCaSlLG7Vo8VEOaKfSRmuk5MxvW1oHPni4iLV2H7He%2BgIhALosaYuhnmkpdmeoEnmWVNHARDS32w3cnkC66IDSEaGGKv8DCEAQABoMNjM3NDIzMTgzODA1Igx5j1P4GS%2FGgoQsW48q3ANrzgIojJx4Coy77%2BvSGFZFjjKN25tzqaTET6NbXaMgNee2YjQxWwgMAK1h9K70s5Tc%2F%2BySVTZDGAD%2Fnoa%2BAfdluDJ7cJIQJRJnhRVpxuYWftJY1kojp%2FPzB%2BcjvSh8xCrQ8Hz%2Bvh84%2Bzmnq241pMGZ7fAeXVLFHfuWvGZFsJDu0r3BrPadW0ZJSPMCVSdiVW0175ywORcW8zf5yDSLDVHHzuT46cWDpOAPYv2NJ5n8uTLS71eH7IEcLaO06WeFXF3DdUc3jCii2dVh3J6c9Eu%2B67vG8mvfN%2BpGw%2Bv9StZ9%2Fyroag05TVpuUNXuvC41SZ5%2BUFb9M3U3W9BAiZ0oVptvyhITOFsKWO%2BIbR1eT0NSgwR%2FeWbwkXRSQ649le6YdEL6mkUIvwik1Eh4Oaz0nDWrSfvmL9OcA4Ek2ai7uREdHh73krH16GZowLtoAiOkD5S5xirQ4LI4Ir1FAArDFMBGwPqs9dUKosU4oWTB5uxd6wlo8Km3RhNipKM%2Fi74lAHfickIu9wcvoOs8c6k7DQHtffXI08J4f7EV2vVVbQQ9X7dmRYYKZbbH1Rmv322TVzsga5hXkFwhrVQ9ZzL9GNh2tZM2Bpg%2BXxMlZnaLdD6Difl5ChlFPhaPCAG%2BazD8uozJBjqkAdtN2snkw2nhn8EXZAy%2Fs4rzWB9XtucGAn%2BsCmedgLvALZmXxukbwYkmSd6CpafJboLKLaQsRpIwOqPV3htDu1CiRd0RR%2BZhrQCdffsVFZELYljfBWjPjyUMY2Ay84RX36GORhqQde8GYSRVJtM9H%2BgdDrhiC5A6Kr95RERN4EK%2FHZINGJNMgjItZZBpMe%2F89vGCsHXycIdARgsaRwaUUH0uQjJw&X-Amz-Signature=73e216679e60713898c860b78eebba4797dd8e85c0862f0063dc178898c135d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

