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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VL7QZFT%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBHZiYz%2FAfqPFxZA%2B2YFjocH4MC3NsO5RnSywO1LwCy4AiBgMXFspU5qVeUPnOn0tEDaZN2k9POMCqx2I5aRzXv7DSqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2L%2BE%2FpNYyAPQ%2FXEJKtwDKo7tklnLSsmRxsJfY5jO8TBtqH%2FRRKIXq1Sm8A%2BavYznXLKXd5iddtlf0O3P1SlRp%2F4KwMuc%2FelPHM6G7%2BvhL5o1vu1%2FE%2FfsPCTEn1Y9h%2BdJ50XwefY76mD8OZKi9xQML96eJ3aO2qsaybQw%2FLJZ0OqmUKMhXOdBeFbq%2Bnqp9E42kgNCEqWQwi%2FDgmEI8r3BfbISWAwQW%2BApJWQkpendAKdeHvGl98Jph47gbFnh%2BvwzKMUPJQF8di6aV6gd9KVOncL1V3HyG8d0h9f2RjaYo0yRpgnoigBbGQ%2F5LVoUGX04myoFWTcJy3CivdDIDYyMpe86W15zCMJ%2F8mC1fu6VHPq8RVqoG0NjcDocH3tfCXHkuB8whkmUVH1sKme0l%2FyA1Wi9rHpPVZty6UjBqLBuvy6KCVOaJszSy6qsmtxB8VLjvI3aaplLm14i6StXgSCZHF%2FXixnmBul%2FQZ8K8G4mpWCA%2BgipElkVEcYUsDN7o3akjsVEIu5PfYuZHXAGDgLAFgLovxw7chWvQzp64dlPKOu%2FG2mPz4%2BYN5mjMoC7mHHrGdR206%2FzKuLf8IWxfO4A%2BSZ00IcWsqiboTkU0xskpXVMQ9g8rKrmSN4jJQfHMUzlITOxt9ggmsdKJM8wnJ%2F4xwY6pgFDa9%2F4Ak%2BFm8hlGBffXYpNWvTASwCY6AC3Eahtr%2B4ouq5FBguFPNcGRO6Qt6kzclWlOqUILBInb%2FMySfOHaexASKy3QiXJC3xZ1sEL6lvT0GhtbRnaDKj24KeTk7k5gIJQ8UpCAnIuEtk1Pa8QYujFn%2FCKdrMtX1SQ4flrV%2BQQgtypoTOyky426n1B9Yb%2B9mUAFWk4MbtXTJy9iZGshFE4GKdEC9%2BG&X-Amz-Signature=7bd256b50b0c6e4b0928e270ad8e03bdcd8e264875525b82a5cd3db662e9fb8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

