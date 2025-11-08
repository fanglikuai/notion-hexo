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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646O6XEJ5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCIBzisrua47pFuS9kSuOU0byGeGD9GYmRd20MRMtJsISiAiBceDLbRaM7VU735lTxlcVIocJmsFmbcpjwCHnDV0IGAyqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3tUvf3QysK15CnDCKtwDIjKg6Wo%2FDs%2FYSmHb9zpx2tgF04wlM8EGJDiUEkL4xOVoSdw2khiMR%2B%2FA3%2BLlh19rutL9yYHMCs%2BQ2oQ%2Ffj4ur5DWk8dawgtmFH%2FKk8qdVQXuZQUKPxb%2FYCV446Ihq8WuvO8fq90D8VysY7LA%2Bwo03WWEVjJeEA6m3xXWLMNo00RgS8mTwByGnOENJ%2B3k9NpfuvQsj3Z0%2BeAYx1K6qS8%2Bo04PaFpW7gBX8sDRvmr5bV3XZl5whGBQdJF3Bew2SrMaPgJvmkHA2TwRRFsml2d3eA2fA2nMfC4MhRgUA9PYBMqlwNBDVcfTCEK5wlZd71tTCd7EZAW8kjD2SqD6j0vk89Zd3fb1NRa1FM6b9GloJJQSRspw5qJskx4%2BF%2Fu9R6wht8GRfMSaCAlDIJGCGVf9jAquFFXFvU0kgoEiXGzbEhmKxDTU8zEsq5fxpNBbQHHGYilRqI5BSZraOYwJ3Rd4uArPnY7OhMOD19cARW4zaNG5imU1XwFBiZQOVQm85u8U%2FRLYXyFPokTOrdwgFqoy7nFd0grz98zCu77TXxmYNk23vGl8SG6DM942m2qYN6gp1G4wQq6yU%2FEDuTFxiXlwGXTdKYMIeRFxdbSzfZAv3jFqDJK1QdBe%2BbG2VxswuP6%2ByAY6pgFHkwgQbxoN4NAK9fctXcqnrtuhBY0dpXfojtvD4vBITB3WgkIpp9M2EVaTZRQakqDqwUK1nAYPh8JQYDjYA3aPugg%2Bdwsj3PwoZUI0IvFES%2BX5daHLyEtKQGkivKHRzu25AfnwyRJIfdaYIBRS4O0zTbS5sbhaKNQUPv61O5vXbSymY1td5EuQN9CvSjjz55tBPzse%2Bv%2BeMnmN%2BjUQIU3a7iuOOnMm&X-Amz-Signature=4fc46ad91ae3ea5c489ec67199853ebc06cbf64776af046fcf8a4ceb1a181d94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

