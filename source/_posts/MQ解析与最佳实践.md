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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IGJHWF2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJGMEQCIGqrbsTqAD0FHJ8C41jaW2E2zdEanJQTukmqA6lwNiK6AiBbwRObZYn3ZGEbuRxIqrBpG0RwkZVo1MkbBJBrkewXgyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMSomKtL85yDrftt9EKtwDZOX1ulUkRh%2B5kde%2BBbQdAHqeB10jb6hvG5ilheeMz8nbMzzKFMVKOxkP%2BeqENmW94EMd3IRb7LUBKVKobHwiouj85CQul12%2By69mj1FohtZ9dj90nz31bFZfvhqK9NNYQR86BmYulFsvZfubxvOmuwVvNpKLnueujfVqnaPZJ9tC48hiuwxXwCy%2FmWaHDNNMXsAHeOZdbk%2B6JDQZ9JlRdBPRF%2BjEWKJA3K3FcRv3CLKWYM9UN0NVSjE4apew25rKni8n9NOoQk4Ln%2B4n0yvueGBRBQsxkO1Pypj3GbUMzCHlGdqx9BFjywh0Y7zZWseWryZjD2GNx3Xn3SjWG6mYGdFaTTrS6Zv6xDCgkZDmrf9oVaQ1mB28l10b4m4cUn1mUmbCKwwP6vzPZL9zSgBV7eKAWhPTyQpjrijwjtL78AgZiWNAlKt9E4cc8IuKaqPrX8dVBVb0B6pbFtHDqAi4xWjc12g003LP2be4CeBDMS33DI3kS%2BuAzLfUBfem1IH4ptPfzTg7lYLfPzZnBK%2BvNth0v0AohANMFlnkwjB1PLZXP%2FhEZ0iajHYqdD9aF2pH05sRYDRipJaGArl8c6BFV%2BxNr4TcqrihdyOZThQWKV%2Ft8ZOB1jzEZNDaMJAws5zMyAY6pgHSbWEC6TwAjtU1sUQpiK6vCE4J3RE6WrtLW4KgWjwHQcrmzSkrTqd2JJZdGUu19GfLX8kSaTYFMELjlWTJB5OrODQhIiBCY5%2BOdV1hGitYImaCnDIh0ETqVrNI3MV%2FbRyujdFqQ%2BqZB8oJRI5FVN%2FejxISsPyFTxGd2OGDs6WgvFW6AS1bsM7506p65BpUaFKmwaKLGcTGpciFRktT4pmKXQA9IAmO&X-Amz-Signature=413ea5b9b7907bbbc2cf74bbdf227cdfba8c39aef4ca5d53a8713ccad94a5423&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

