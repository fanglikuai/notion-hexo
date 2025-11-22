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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AGXEHS2%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIEUDrShfwN8EHgX%2B768Ja%2BtnSzH78CWS0UJRKjsHcHgLAiBUpofk8E7pv2V%2B4Xud4B7ouKIFtSNUxqQ38ztV9aKUNyr%2FAwghEAAaDDYzNzQyMzE4MzgwNSIMZ1m3OK27cIz6AAw5KtwDX7eYGJi0ARzMuRPaMODG2eUd4E1Ge%2FyOxQOOHdS5NC%2BnqYBakz68aDrplQVLPSM%2BO3UdM3QwlERRRmm3PkmAWMLgsfx7pkzWfUPdQQZX%2FXub48hsBaHVs5rc2SmFs7kRxEd3KVCv3IbZ%2B9qhk6CexUV4Q%2BuGSVLsT%2FWkJbH75%2FXYtRgQr6KXyj8EBgCX1vGCs9zYJpTcc3aonZ2D4kgQu0Wr6d0D8u35mLKCEDX9sDcKWhewwQS7h7PWh33Us0gXUv15yOj0EzrVAZps%2Fz6NNGTBljgvJ7oRUthzmeqgbA9rA7%2FAjkMX76l74ObYql64PXHMG90VWzDGx%2BatLDTPcx38ZoancExspU%2FJRnIZgnzbeCMQzPTGZrxdFdJ8wswGJwLFC8ufqNR0YtwNVEbBqPNV62r4Ozjy8KsfXGAygtMgjG%2BLIFdaYBbR%2FmJyZDbOAE82HV32RuqQAYFVmvpYnXQm6FzY0m9DntUc9YPt1KESLl5cEOBVz4qeMyZNXIo%2F9mUDxQuwQOtu2cISkDDPhTVzeN52pfRJInLHjRT%2FCOnmyYoklHzTIlvB4EiYiomivLvRVw%2FfgSfwkMxX8P%2BWzh3bfdser%2FXT7NT%2BVxPy5V3lBPQ7t4Vu90s7D0Uwhd%2BFyQY6pgEYsQi73Kz5mc8DIG5fZK%2BMoEt4cN7XH7g3baxN0Lesf%2B%2FCIz2UdM5iFtyB%2B0OyOjhH05caFd49LWLs8AACJQBndMlutSRcKHh2GqJu4ZBlp3SjOgLQMHEAo3%2BKXrcWD0a%2F8VIVpYrdudek7OCjTU%2BdyfhuRGmRj3%2Fxtt0qGT7hvPs9ScLhCovXTlqgx%2Fn7uVdZYvHp05IZ8KON3GvOknMPi1vBlLOb&X-Amz-Signature=3235cab74620ed324c90fcb5a08cd2e69cbb5cb6338bf5e7d36c03cdb29c7da4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

