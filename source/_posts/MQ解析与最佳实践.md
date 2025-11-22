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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4FGLX63%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIDyQ%2BOJbYWAQoOu5a9mrSu9111cGefZDiuV4p3t4AHDgAiA7G8k6pbhFcfvjDgBdM8Gcztwhc9cGSZ33jZnDvjFEnSr%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMCeYvbbSHd%2FbixWfUKtwDb9EwimqbuoBc2vS1WcCBQDfCD7kWgx%2By3xZThnBjsmT59a2YU0v9yo1NYik0VG6C9sz8c%2BwTtlLXWcpyJv91cCtK31iZbjXracUJUnbLYg57UeLRqA%2BxbSXBigTO7QZhG8wcdq2CQ2ca889U1AECreyYW9fYq5Dl5uhzxP6JKtLuxgv5yzNvmbeipabDMnQEeMUuukQMJ4%2BKtlXaXy5%2FKGp0OQWBtV7dRbtfsMCIFFpPS8yF1aXETWH8mFYDPuNbMSRgOdTO%2F6R%2FLz4QaF7hZJ3vbezaXXFgjYTAfGqZI1wu6X2BnQ0TXBruotQ3pvBXbHNsoE7VwUqNGQW9WX0ebq%2FvktJyS9N8TgX%2FaA414fC35aJ96xrjFLFhQdMK9nsmeCrTA5UtbVSvqUm4rBhyCkMYGcg%2FbV3jiBdoxbjSLzPLWKzjLdCxwRETBGuMqYVqnqvgF2PQiWlzdNhaTA4kYSn8PJn67OexrFH1zr8No%2FwjvBSi0P43D1je7Yu0NWopYQn9FORlSxSjwfGJZ8C4TU56W4rkbdxEq5ELYx2lm0JsJMqi3%2FdFuzvdGhSphQZJOgxDOUFv75JTDELcZvw2lCK3YhEEmJuC0ytJX%2FphXLM4TC48m%2BxI0q6eGzUwzYCIyQY6pgEPUhjLtK1A2e%2BCj40HWelIbf1xz8l4fjkjBnBHeeKLenJLOlpAxLd0VxuZ3AYkEVvEmGnF3WFhr0JQ1f9ZGMLxuBBr8FRjGswSLS%2F72XJHNhLCCXFX0gKvqMbp3dw5obJREsT2KrNy7vppiyOU2IJMolRP48GUgQtkozUAVXXuP1bqTsyz%2Fk2AUOpCPEF7k9KYI%2BD0k8MFlHGIjLk46EVE%2BWzxe1cu&X-Amz-Signature=ede1b72a1b6a10095a9e15c33db3c5fae34097dcfd7c89ed15ae944f8fcc7c73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

