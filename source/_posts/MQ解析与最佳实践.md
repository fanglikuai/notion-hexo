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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOA6Q25O%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEOVj1X%2FlsOeDS%2FklBd9JPDmjF%2Fuuw%2BDOhUAbEqVUv5pAiEAjI4ILFoPd9r8eO2uLbB4JBNDVfkEnpjQCuEaH4MEv1gqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFopVwTYsjYv5nU78CrcA4zytWFEs%2FNM8F4hkx%2BnnH6QqzOes8dZRwJRU7WQuQZWDK1CPCD4avHOPbMOJ3zlunjOoicav8UXCjguPs89TBEbt0wVuxgOmYygCit9U9rwrLEM7kB1Fnd90C%2BPyd%2Bsmum2HUJ%2BDErb4kiqvtolRNjfMKkWPd2LwdYOrc4Zu9AyrOs8MU5GNxJl4EyhNs8APOgGflGR6HcUEauUqdEFv%2FAJZiFL6fFSUEzIuQVhpdtlChgSB3W3RkxSdS9aLOPRG3ZXteqX2%2BKXg4JUqEtolNOXMgEnVEzcLn1k%2F9AG46NtN2LcYy6ics8GVTyy%2Fv74LDLbHfCaHt7nMh%2FPJQ%2FgeQp9PL4LPi%2FjI6pE2nwVfUxTC6A5RkXI0asLkyvf%2FlNH4bRscNZSIAtuY236xuWHcOQCVuoatktOKfZVpBeX5xx2lSD%2FwIPokOo189ONEsVywG81HQMMZxZYz1toORyyAiG5HU5WzzLdYOX16hPF826zH6YLDsvsPFvs%2BY9h3hajmGiaytF32kKutLHD55EN2BAMOdRD1VHwQ6HU2aSQJ74Cati%2FfNjn6vF6eLKap%2FweohoGVE22vYxlfRs5RcWzQ9%2FuweSPgirfcMUvMsFqmw3GUZwI6psbNMCYwE0wMJC9gMgGOqUBiZYFhZ3wGXiCkhRnZzE1GVzESguhcPOVDZB47AW37wZ%2BwvMpxuAM5DD3DDDhEUGKrwnF%2FCxB5q3w5cQPpPl4I4p2vCJ97dWixmBdNq6u3DzOy8h4ozBSfaUNWdeq0yCyN4WbB5FJdeEWb%2Bhsj7gfIZjQByfwRNEibFVs2Cl1PQFLJjg3csccTa16X7yQcKf%2F18fpqTZiRQ9c%2BTg6%2B2Lsk%2BC457Ko&X-Amz-Signature=18d6e928b4e26dd8421acd8758b17096d5ace41642628ddedde17c72882f0e18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

