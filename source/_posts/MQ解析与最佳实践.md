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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RIM23B7Q%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEx9lC4915yce5%2Fx5Abt2nRcSqFZji1QqYb0pky40zexAiAjiNGq6Rm2Je648d2OUgu%2F32AEczZKmY%2Flr9K642ScVSqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FJMWteYToHr8voCyKtwDtQDLVCZt8RyXZECfXF1louGHO8wBeeBJWpXZHIKSnZFV7e%2FPEzeTaGTXPViFcas98dUeKiuHNGRmtH50qE3b%2FD985rN8V93tDVSUAPDiVrgBamcnuMTkbjDdqTLvbJ2bl2HRUXjS6Ae7DJoFLH25n%2FH5tGOxEfen4ij1pfPe3CKEYb4fJYRJ5peL2EwcnKOj56PdV0eXRHIqrHLfNt35znRqS3BE3mEfa%2BdFgYBKsCLo3Jrr9W569BZFf7XpGKnWwzuEkpIPgl3NIGbKs0Iv7AI8eiewJbpFhMvp0yCnVUNb7RR%2FNdQxx5SeSLFYgotgyiH3aLBhYH9ILBlQv5OeJOWh4RPA4FaCMyTDrBz%2FZoQAt1YV9RqmzeFJQ67ATdxz4QhWRRSnabyU3N8RmQb%2B3pLoAHHJAwAeAljMj9SK44bU6IspH08XJ7LZ0i%2F9BJQyQN9dSG62s1C5b4n9Ouu9%2FbkDI%2B20Gm%2F7kmN0xUH1N%2FUac3GEWB9sAZXwiN1pxo1GR47RlHsAPU5I9NLZp5QQF514C75hpOeTYVUfmV2WIFtwC87BD9WKtiefNhHq0TM3VsfbQvXwgJhLEm3V7ACmm%2BGpWmMKSNGgAyan4dEtk5fAzaxz3N1%2FWmF1k9cwmt6jyQY6pgHVUV6Dvo%2BG8x0tnxoxLnATPNS2H8NSUeLn5jwde%2BkYGnpVa8NG%2FauehDj3aWIs28uxO10C05CtoNHoeZpd44eiwGtsBTBKx5uy3Wb%2BtS8OtX72%2BB60TLRtVWrng3x382UU8utUbcIMSk1UkVWdfleJLACljI82MnALIISaqYFpFrFWGEpYVro%2BU%2Fkrm0xr4E1tpsIUhSUsd9aM5WkxYMPHVQY0O4%2BX&X-Amz-Signature=79484db57c666d2b22ea3879bb5d64f1d3eb52f3b3a3305debb5b6a671019481&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

