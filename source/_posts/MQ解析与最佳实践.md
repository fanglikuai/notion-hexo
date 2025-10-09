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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJIQDX26%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T160235Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJGMEQCIHYoWyzltOvEeGjHXUHXreOgDWqDbF7LByT3Kym%2FAm2xAiBi4KRJxfK1IOu%2Fr%2B8O9QDjdWKeIOrrWNmwx9HRqthhiyqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMylFC%2BY7ZRaDenvjvKtwD5u0gs9OP378x81jVt2F24IbHlKTR9PXrs%2BWP3X7UFHAlG6y3U0SgJzfy4iZo15MtD4KQDdcguG7%2F1%2BoHkvHppcu1nbhqul91XNV77FMS4mo0SgN%2FxpUbKKlRyKPR1sLnnqBLvPaNf1V2wNu2ZmQpWUXnnG%2FOqEe5kCau3w3cL0uRmHIXyjEj1ChOcub3RVFroGr1yuE8CDOTuL%2FqyNGWy0%2FWih5xINzBD%2BDE184HfLPraaLNqLj1CdvQbAegyjh4gu2kWBRMpRjsQil%2BKrY4UvKxbjbBmVbyy91jtpaBexiR3aYmKfdn2vsSLRPFb%2BAvXGCY%2B%2Fg1vdM9vg6gGo0gC9rOYJHqXwwQ4kTaTSHnX5TJvKWxO9hniysXq8%2FHgfSA%2FTRY64cCY63xH5A9A8Mhxjc%2FZDA68AI8j9ZCyHgalXmZKITQOxfcck1ZloGmwLoxmsz2CkPCu2QLN9oKjMvIgMJprCF9VdYiXlc34rnGut7dG1e2eCQpzP1wWpudcXcQ1%2BfyMAQXwDOkcCcyAj%2F2oVReplBAma7kyS3V2k4ASurjvjDaRWQxL1FuLkhFHWTQB6hcRtO4d4fTJF9IeJgWuqEjB%2F%2FX4dGbDwMXyDqfBEioABTdLVdjNn6sg3MwmpOfxwY6pgFgfq1EAnzKifjK1u3CsHG6%2B3hxhF3NiQ3Qh0ke0aLR8Kru2fwIowGCNg9iumWXCGWoUYaB0A7fd5jrapIpYk81Aasjw4bi1er4%2F9ChUfn%2BQ2%2FLcBKjgpkg6xs95s6ycLypDkSjHmun%2Fv6xXg2PRFWdXMPTQUobDjfF90IT0O4mPb50nBufeAvrzlfKOlS5cF5OmjdAEies4LJOYjBuemLzGTyYbdgB&X-Amz-Signature=1bab8eb306a62821c415b82bcd585c9a5e1e8eb2a1efba192aa944c21af96568&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

