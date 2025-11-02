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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Z44WPFL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T140111Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJGMEQCIA9lXryYO1%2BIJr7HQW5JpwKNwJudN7A5sAc%2B%2Bd04nSWxAiA%2F0qaCINAFN36y7v%2FyK2ma3TFTDFEG8TaXfE5ms9uPfSr%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIM1SIfSNWOHsdt7iCJKtwDhm%2F9D1oIS1M4o1wfq0NEFPDTQ2zchmpTbhdTLYIlUXUmRGGH7lefVDWeNzlkKTR7wwfPTb9XIGZpW9YsZXCHBxNCtu6t01eXG%2B8ErsIweo728pEvWCjnns4QO0Ahcp0TJuZ97Wu7qHhRz5vkKceNZBKfSogZrvn0NvzBdKJ5KXQsPAj%2FzTDapRwhG5ZyLynNOQ9XH9Uu351CFh9k5t%2FD74P9LCFUEwf756bVF0ekuLaRZwrWoQXlHeb7pe29MrX%2FXFmjhS4qx5fRIIZEmtDidAr48c6O0sOEAXqYmiPgNYMrahky89Ntjd9aEiqcR1Ukl5tMI%2Fq6OJrDyfQ95wAWucx0eUwUX07TaCT5dU27XmKGGA%2BM1qB3Dg6A3izhDOpADSeM7f9KmDx8wuUZggp3pFNUzb%2FFlU0rum4pV6LwSiBY7NvE7vqU8PFzWM6R6UuV2iFtAG7X%2FnL7SHBIDomcSAtgF%2BZDndU%2BZ5hIET7uo7TsAS1qIvfSVTfLf6e9m0%2BmvkDgK41bdsiJ2UFRQVvj%2F2cmM%2BHoSwnxAMdFipWqmyIeS%2FZGcKew%2BZqzjZ%2Fi2Bd825WA6KkTOnPEW3JJ3b7Qib3IgfZhEW3HCOzrWs9tFezzlQ6P64iwJWwBKJsw1O6cyAY6pgERvxuzapFn49cafC%2F3ZpXHGMLt0Ew6ehURfqNXUuw16u43EjEZuQltpxcBx2T25jj4nSSFoLxJ0REQv25cz6%2Bl0YRcoHd2xAizDsAI3esqz4IMVuaEGcmaEoas2NXeLGMsZ5RgbHEMqOr1fW2O%2F%2BNOJKimD5XCMrs4X0xYgXcpRpKRQlprC%2FhbLk7AnZUn%2FJEWg5jCMRKmTTzuRpJWYjQa2fUTWnsV&X-Amz-Signature=c22ce322631853fc6019d6eebec64e9ab82a53e462dda0a245c0a83fc45d7919&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

