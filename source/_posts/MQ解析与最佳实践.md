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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWLAFV6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBN8AOx5t3LWrxzsmGXsCnKEALDhBdO%2BvU7JYZZ6Jm36AiEAqdEQfvurez8ptgZXgDqTzalSffmSK1ABOvH2ZlB%2Bxjkq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDKyXo97XvHCwdpWvMCrcAz5kOoIE4rehas7yU71omvLOd1pT%2FPn%2FqpIhicyOdeAz7sy9JgyRou6fCmzlbdz2skQeVQc2Avv%2Fg2S4GqUI1px21w3XQAqKOjdcQuwsMsYyD%2FOzid4LHbZPAFHXU%2FJEV%2Bh6OhEeKF7dHdmKjy%2BRiRkSl3ULE%2Bss1zCPZ5wXjluMvw93HaBYHLQcNWpnqEmAvu6WAHukWDJa0IdhQ%2FdDMcekVXYCyx5I2JMvd5rnPIctG3HBSEKmyVEDQLC6L67fiVUCYsi4Wmp4aJXp6N%2FHnymBA1O5CgYmS44EazmUi%2FxUYElbyhTbfkk1NAJfVD%2BVD8BFlWfdsTLn4dux%2F5SSh9H6myaxsDRcaTA998jY69dPZOxWca6zfQOcNrHiBWeurimi0U%2Bh6wNEcRprXKsmzzHhbPyptPYUja8Onc5gblDiB%2BEHwZT5jJutef61UP4s1%2FAU%2BGX%2F48Q6pLQ7XH%2FJQrP7A5CjKhlUpOg4PW04HoMhBXIFUkH4o9mQbXJGQXXAEBWiYNYuvsnGxmx%2Fd%2Faf8xN9EGRghOPXt%2BpPboL3j1nDLPN8gMzhKmBqt%2BtyUw7jt%2B9MTolbx1L%2FPuvL5TtfAfGm7yrCZV8DEn%2BNcbNC7cqZRyiec1VpgPYUQnyzMMmHzsYGOqUBlpnhaYX7gh2e5AU%2FSZX%2BwnXiV707tOUEQat63N9rZ2a0kmqpGXSQGsHQLpaOEPUoLdVJY0cilLh%2FDSS3GXZKsPmM64lKcX%2BO6iSHDuQSbt6egMrGlplBHCULo03B3qv8cVLclrAXEG1UgaW4QCiaZglcVZfH0q44KIVvvXbnI6ivQQ%2FYmhF3kaSXnMqYFGJ6X3rVn3NhhaAdYrMDuGso%2BmiuRm4o&X-Amz-Signature=edaef09ac099d4b8d6dee8cbc585d0fa8888e5e50a922bd28b016fd155a5aa37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

