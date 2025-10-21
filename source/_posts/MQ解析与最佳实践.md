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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SJHTKUBD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T210253Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJIMEYCIQC4gq%2BjhilUiH1scIY1HT8x%2BZdKZNb7U1pxCYll%2BIP6EwIhAI6uX8L2iSIhxo9wzDKwXOdodb942pBUJ1L%2BfEJpmNEzKv8DCB0QABoMNjM3NDIzMTgzODA1Igwo4ShUMi%2BojPoGoPwq3AP3wxNywfGVl19G4ywZplrr9embDndBMqAJ%2FoMyYKTu21h7DYQXCua14hTjx7fNGv0S91%2FH4Cz%2BoOoh9sj0fRmAx5CWt5sCXSq3RVRhdojHjSOSSaRGf8fc8%2F2fdxLOpSChQz0S95X2RT%2FSQ1FvuwwF3pWHRuYR7ZEIWWF9O8DJiuEukI7SGLMlRh2URzBpTkwBLzt6fCQNcnTebDFMy6ZnDrwbUjAledKZAgtRLYwpNfOwPg2cJ8Ti6CgCuHrlUtGBqpNUOSC6iAZqIMTKeiknf0b9KAZ9u%2BAcmNTY1gYOM8U90uVwGmsEY1HUt%2BXr4oyR8vtBVweqEbnT5AyT%2BrfkdL2qVdIV2hC39%2BkMPzVQRW5nbU5FpsALmZcmB4dlFKgcpFrsH%2BIyMo8SbdHg1T6HZEKzXs%2FljSVHee%2BVdX0T2nG7szNJi5Sg5vZftY6pNYGdgozhE%2F%2Bj25Xul%2B1p7mW0nSlgatDVHzdppgwtjkbueSODP8XDqUDMOiH6maLIB0EflZrsmkvMcGEg7rXhFifvdaItqrYZ5KsVrv3MDTpqcPMe9k48EYhU4dJLOgmW0X03rrYKukrrFTXGgTZ%2BcfQfvq4glehdYXGJLp8clCG4Ub1FaIc6nB1wR28RZDDL09%2FHBjqkARWOlFJw%2FY%2FutAzsiazxiL%2FFSUELw2rgTCdqKUJtkGzzI3QscsTt0PVN4zZcLUrEPA9p%2BzS1hkDyCHEWsKVWPycOVwI%2F%2BOEe99VtEPgS2DJVxXFVa%2BAvRsHAZedLXpJOEfQgyHeVZ5VIINum%2Bw6SNifJ7%2FvWLH5jU3mgwjVzZz0Ln185gV08uzp77usxVO1fiTItzvPyDKyn3jhS6ULYm1CCgVEl&X-Amz-Signature=76d2530912acb698f61357ca06848f4a777be42289da2a3b8b3528363311ee00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

