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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7A3OMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIClwxHp6ZgmiznRIWZarCFY%2FmCMK8U0mgc5QwPrTEt0LAiEAgoHV4GxokWZOVqafks7Uw%2BRfkRhDihjZiWeO%2BWkohZcqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVEZv9FEGH42j34uSrcA2ERTX2AZt1pLiRAmZHTWyBsBNOU3%2FOJaNRMRj8wMdqZ5jWccoH2Wx327yCsCdza8TjzY0H4yRG1cb517RwlxuqbcvqezDGE9bjg%2Bbp6983i2ilS7PLPb6LYceDWn%2BuR2q1tjpKCLsViE12foRASzOypsZMUkLCAaTOfbigZ4%2F52w5QwMcaCJPSX8SVBEWFRB4qVIX2xlYVWRxDbi4liq4dtCEWZ2xpjuQskALPi%2FUStLYPPVFUgcE5zcZEBgkgSoUhS0sDlxNTrcEyry8v5RhQpWWnO4f2BsdVRT%2BakRvqaOYp3Ep20hQs1lKLnBbO4OXJL3O2Qx3%2BcsZUMjJTr6mdvdwzHJeP1EN6LVeNcBpbiB3nzZu%2FRIamSK3Tl2S%2FO6JI5BewKO2dPvPJEgczk%2BePizpHAF87I3PxsRUkGG1FZju95UGAWqzFSKJyqZ4iLjIDq3D77RgzoM%2Fz%2BTnlQ08M3ZhnYbfQqcE5JKWucq395xf6L0NLsaFEo0SuCGv1kLNykFPBW%2Fep2n65okzJspaD7ID7M19%2FD3wnVvRnaPJeAxpTmjrxoiQ5y4HlPqQu68Vb%2B1kK5jxzOIR7Zh28hwRrzu6%2Bv24bY1UuPw%2F2Lhyo1VWadkUkPP39WCQSrMNHvvMYGOqUBlmq7cPTowfyP%2F3UjLFzTbAFo60I%2BrUbLSnqon3hoZVAb1CIK%2BDP8p%2FWSJVIrG0EOiiK%2Bkjtv4FuYiPsju2Zr2auaHwp0naT8stYILD45nJQ6ditChTXBP99Rg%2Blg4RYY2hoak2xLzYtH9iKqwQzzmlYkNzdMNxMggyModPyEIv%2BMlxKQ3UpI5%2BKZPzc6U%2Bxh0fpRmr0lviDXQpKftdd2tnLc%2Fghr&X-Amz-Signature=f07061114f6326c0cd2d42a23ae62f0316762eda1cd2cce5d5029abc369b3a55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

