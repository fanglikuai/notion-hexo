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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666244QSPF%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDI1Pw0HaS6qItQmr4uhXlhbDHBXAt9i0nqAulyAVuZUQIgK6zh0eZARwhmsg07TeVxW2hYfh6A%2B7RcqhDJx9WkTw4q%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAWFS4kMsrEh28b9gyrcAzGZa1y0UvQanfiVI6Ium02HMcSBDS%2Fhunn%2Bd8StGB34CyteJjIYkALzxvuNxCYPBfWOLjU%2B0rmL7obvBABdFnpk301WM09Dx%2B1D7I%2BsdqwcqjxTFJhHqcjW1E8EZv8Vo5snj7ivRgfv39Ixkhv6rQTX7DTtbgV217iVA49PZjxom%2Bn7%2BUGgcpMbVsVXWSC9gANcOmsUGGHKG8K0K0JAYGuZ78W7zLztABLoxsExz0HnM6P0u6H8XM3eZK6yd9eXI1wunjLN%2Fqrb5c4FKNksW16n5LRLBN6aXTM6IpT6Ft7uTk59od7FrWVrRcoZFKIX%2BMZezN03SyfOi5Oq%2BDmib9ArvJCTpyCjXj1Qa%2By4K3YqRDFNBUIFotHBDH0szA85R5g4Fwxnm7sM8s1GjdnZtDk%2B7RQvghvX%2Fe9wh6%2F4Aoq2HNQxFJm4AIHcZ%2F6FV9DHHXF3EGTzZflOsb1a2FFPp132GeZ8uo1HXk1bVM%2FOIZ1MUO%2B81kcI37OWPQ2tlgWoRoDNx%2FsnTEgmMFCW3hFA6Ksz9wKUb5RkuPda2kd1JQixn1%2Bql3ll5Mr3eKWTC1LHLpQ48F0OrQSdKaJD8LG0wRuxPS6pSon3vkBLuL8uOd5rlkDnMx7v3aRErs4bMPKjiMkGOqUBURy3o%2FojaFOGGyVvmRONMCWgSzGxLblyG03CiW0cpeNDaIRJYsk%2BbqewicTYpP%2BWjxM%2Ftuu4aqxkCLmzYh%2BFkTFy9aLv7PeXvHDn9QnNLAYUqNNjotPtqtoNx08KvY5x8EQG6zIt3CpbzRcNvzStlOxtgYSk5ecgQFSez2LAPF6HQ7KqDVNJjQC8m8yzashAFrLkcdFGpldSEmcdqYhp3r%2BoHarr&X-Amz-Signature=0768b38eddf7950d6136f598589922fc2e0f7bdbe195d2b31499c7a76ceea730&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

