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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJPRBO5K%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T010051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJGMEQCIEUhS3lwQrq6SMEYKMvyZa7PZFfZ%2F9mDSy%2FMz2qjoLUsAiANEzsnxicyxF4EivMnmPpcnGfsVVkDRL1hoAFCYE6lKiqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdjoYkdTjO4CAfa2EKtwDEpbP4bUYhaADqitFLfA4R%2BN7qVbahakaiTbG%2FnRld4IBKEWj6v5TRPeteN%2Fwm2icCevVZdsiR8XOaw%2FEaRxvuCUFX32Q1fyNvUQTbvxQgoq5KHie25wX4fcyAZdh3JSP%2BvxBW%2BGqJYJwSG70I9MDMc1ATfALw%2F%2Fd3w7Sl4FXS7fkZlKYVQVmwapGBKPM80kI1hyrI2rY68ohU24mYqqH7cjYGdRa2RXWNzj19otDcQuleXpGN0jck7FQMJLMtzQq84ZD%2FgekzUbN9LeoMhXQQ%2FkUojCzKR6smXisVMEHjmfYVrtIwCprlz5Fa9dSsfx2WbTzuf8TIXp6BzaAI7Fq5ht1RUSzY9i6W8OKDxi2A8gvdqk7G%2B1yPOEiRDAvh%2FFQS3mmUABE4BAoRiULdJI3JVWK0i%2Biz%2BNMWB9iF5f40GBQATIzyCkjRtFIglND35Z%2BRfu2xUNf397Fic2L9zy%2BMmGtHlX6IZ2NuqlAT1ivotnYPZq%2F8qSdh%2BuMl5DhEgZtzzpyYvRcNv7dQ3Ew8hzHwyepLc4Li3QDXahUEsn2QEQGpAZ1aS4OnA5mvPSXJPB%2Bi0U3PsrU9NTeFbsr42%2B4hhXAcz7RXLZwihzkOSyII9tVeqzUgZBGG6oBge8w%2FMCmxwY6pgEynj4Z4gCAPqKZrr86dE9m3eL3eRCaeLAe0c9bQIN5TBona5XIxyxH7E5Bl6wXZq2I2%2FQWzS4InCtj%2BHnvPXd8Z%2FQeUHy4EUcGIWJWD3OYA5Caqs6mcp54%2FYbVCTe53WMETPXJW8Qtmm4014gX7JTUWC9vwN5sDD39QpQ54%2Btfn2qNmbcCj3iTE5qdtSgyUdTgsnnPPODRZrCyKs%2FmPzXmPNqyDiTL&X-Amz-Signature=590e5ce14d18750e87b627ca9927603cc8fffe5e4e100155158693ae1ebc4297&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

