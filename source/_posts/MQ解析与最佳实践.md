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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLQDJLI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIDuj2KcDW4U3G9%2FBZ%2BhlVYJ3aPgSejf1VFpp1H1slEarAiEA3A%2B91E%2BPM%2Bl614iCKRopdmr%2Bhj3r5JAde%2BVajMXpDZMq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDAV3%2F37px%2Fyx6qnpvCrcA6zINM1%2F8Nl9wvtDRR%2BNeJ8WdwMA8io9B5mjIVqIa4cD6AeR5YjOxKluac3y9XuYYlYhz9Roh%2FfTW0ocWkV1xnx1aMBCVriuPsARjZUgk9EnEd7jSuwUkQSjKsaWGADy4qo%2FNMsXtzFiKErcjoevPi%2BDhqyVsoT2Wl6DN1nCWGf5Up%2FfI6uEvGJaZB3kP3Qg7Zyz7aIIUq1ICW9hBnRPKT3ibXEWOyfwAwCE6tIEOx0W5STKF5NPjYGUWPN6NWDCjfGhAtoRAwAYTcq0%2BMWmR3sFMZKsY4xWFL5GXJMnuyZsRq9bflpfJA2u7x0rDn3%2FQgq9gZZ%2BWrIlaU8waOhSCqAvBLeM0h5H%2Ff39urIzr%2FqDJYkVApFMVlI6wO96Gol98fHMqyFvx5oGGis%2F4FhaEhN9ccQaQBf8xC3dYlPUuMHQ2vS0twYVPQISEDCherKkBJNme1ZDqZWqT4%2FxkroZGbmejqwS2g1yE0kCvuybNH3P2NNjHXl8GUrZKlRKc9BhsPQXvU93b4HA0xC%2BHXDPlgkRzCPAPfHf9WCILVhUmns4P1IOrfa8PCrpc22FgQlfv8%2FKS7EQDF7tNq%2BVzPseWaDGuTtwwD99zjUvsYjKy62YEDLZgLo1jKCpRk%2BsMP%2FFiMkGOqUBrtwrht0Jp8Je3hB2qiDYhusStsi2UAJ7VZV2lmSTeb1e%2BVMBOG62I3DBSTrRqXwgdZH%2FBQx6bIehluisBRy90blCTu9k62%2FoUOijRqq0AsyobwizhSilhG%2FS%2FSUJC%2B6f5BIbmoF3aH3gMT2jBtiQfvbZoKNlevS0CsHhofmsLasOA9Wr61ERhJcufL0I1sR7MXfKlNJW1157CJ8xGjy%2FaMWiQaxC&X-Amz-Signature=b732f48f5a4a51917f01cdb673561b1783883eeaf559d9f686400321f8f50114&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

