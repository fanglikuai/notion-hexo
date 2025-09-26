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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PJG54GD%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCICRhW0pUMjQNS4Klkm7PxFfNE1SKG60yW1upk9fuUgL%2BAiEA3AD4oPc%2FpdhOfWrHA5xgO1SA%2BDfOQ5KMGIuhAGN%2B7VAqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEj00aWn70Q2fESv0SrcA94A4nyFub0EQSXl7Genarrhctr%2BThUCiecTg5ztznfPR6a5pqPB67dIEcd7ncoPRYCyOBJV2YNyC1J5CVUSPd1If2B4XqN%2FCoaRLDTe%2BZUMV7%2B9SRB6V10Y0ZDr7aRP1GYEdLCOjg2iFlVUYrTWM4LbeWqJTb6KeaYVTOzfQ8h8yXBgkt1y%2Ba8OZ02xCSyeSyfuWuFUP%2FbvT7TFJnN%2BrD2gyRg7LcwPgjlV7RzFeODsFcyo%2FGOMUBeOCR09hM8ReM55hKTThfGYVApjJLAB55sCbjrYi2M1o7AOOlpcykmmIuM3%2FxdAFWAZYgjESUaB4ErnQ%2FZHn7wYfaH6ZMSCH5RHtFQLU6eL1JWC1lJXJdy6Uo8u%2BmXV7W0e%2FCTnKxXdEiHzq6kMijS5GRVMS%2B2E61StuGnq7Seyum4DZqI0L0NwkuvSzUWivq2G5h2H1TgJTtdNTClr%2BYkf0IDPRhY6spi4FM06sU3D2u%2FqaiH6jc7bMHbGzkSRuY3ryeO45GWOz7qrBVgcHpVATDOWe3TI%2F6NHeoqSQ6HAUfmTgRFeK5L8H5oEg2FDKe%2FBxg1paRZCK0INXVns5ZXFtr1tZUtmndKMhLgRacH5lR3d%2FLTSrTIy2YLHtjZdqpz7tJmpMNfK2sYGOqUBNK38f7mSPkBCmT%2FIlm41bFxBUeyygy7jd8WtLBpEaNvZz2Ojb52SFypPhdGh4w9qBJVESk8eN3l4sTqkoPLPu4lc7vVznuqJXQTRZUhnGMUB%2FrAKU0EQYL3hzo%2B7FwHuvBpz8BYAFkkZWPpF%2BTSQD%2FCWi%2Fz4%2BFJ%2B9wc88V%2BvXIdXJgrfRyNJoRcPuhJ5ULMmm6CnzhxYzWbU%2BVBN7Ug7wpNkzsNo&X-Amz-Signature=00ac0768a05964784ebb0dc0cdef8a810e4ccf0cbb5a57b7553fdff77ef0d445&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

