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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YE4SVVNI%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAjhyuTWcfUQ2Zazo9tn2AUS4iGNpW3Zl%2FFQ0gmqErgzAiEApgdEK1ATfHtARBieHZXu9GzKNUslyBXRuqNbeTWN4Icq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDJOFqEgxzBPeizYP7CrcA6m3wShbC%2Fux6oy7jhIBUO9iANEblQLvH4G87wnRnGGUvmKglKAPjY9O7vwRoRVdhrx7pPAPkucFDMYdMVFyciquDg9pvPzntFbM2pgLTCcto6PALQDfVwCtHJfufG2mxUc7XVu2wcxWgfLK0F47Xy8FFDWlEls6JW%2FEV0zscKNQOG9lunR%2B21fJqqXM7nH9CK47yNVYUarovI1FxrTD8EUKnGC3xJohsW1BP1exhTnEn18hBev4SsTcrww9ow6xbpM34k0QEEsYk0SUexFHEV0h4o35lqweIq%2BA7Z9oWDEzqtcN09Ez5Gn4KSpI6jW5giL6BTiWgLlliMWKYa1wJ3vb0h9Md6TKnSf4cGuSVedFVhJOQfxru3h12wNp0RDGIo9EciF7FTRvuqFdV6Y5%2Fd8yO7F%2FwjTAVbfLymMmB78WrLTZ4LEvXR2h5mzap%2FP0zUrTTqzLqnJyKzFf0WNaBD45WkRJIVpH1BZiLEQuVtTj1%2BfOLXkzBbv6qc%2BuFNBxNeCVpXyz8TdZywyUHQlNU6qx%2F8S%2FLhiFhRSyRtRELPgprj50imwzo8OY0y6hdoWRWZOn2bz3CoqrLjTaHQ0qoK89rxQiDo1Oi56T1FG%2Baa23Sdwg5W%2FX1gYxb4GrMNfhhscGOqUB3Gn4xG20bLbNHlqcrTxmdz5J2oWDPvIaFI0gvPZNORuhsf3%2FKKLSMXRNraTfL8l%2F5IEYHvCaM5TNB5UVkbElJ0Xx%2BzxWyXdKCne%2B8NuewvzPapRC20K7ruSkx%2F%2BiUETTqZEKh41wZBxpsItryxHetiO%2F7%2Bdjn7lW3MSxdDVWmyAOE%2BjSpla4S%2FsaC%2FDOj7Dzm6tiAHcCxxoQCWNmL0wOUUqtLcgc&X-Amz-Signature=201d3a01b8061ebbb4c9a60f75f3dfa59f5611c0dfe85720d6ff5a83a2e42029&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

