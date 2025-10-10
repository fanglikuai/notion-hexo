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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2X2K5HX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIGFurbuZL%2BpD%2Bd2FrpoIxnB4jE1wRcr6m6nA3PUV2xR4AiEAp5aGftoSTuNZEAxI4N4Y2wYz04BFkdj0VIpK6KmAmsgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8MQeGO9LFjPS1myCrcA92Zaq22WxuEpmDSSQsDsB8Jfsl%2BFsvLOiBAWWoFaeTHfgyVCR%2FutHubDL6LmElEVAhBg97qcPRI7v%2FR2AobNoOVqbr2G2lYdzdUFkBPgyEtULLo196pecU5aNlBUzmU6AS%2BuGixz5PElgdYbqMytsR2sGCnY6FZb6ag3HEuG3%2B2z1422vEWv3MwXHZmbnAY6O%2Frqag%2BCLdd%2F5mat2vBEyIweQvs77chJ9iKw3P4joG8xMHs2oPRceKleMbTRIMV%2BJN1bmE09mzzoCQXpPFZZ%2FjR4jmDZLOVgP5hZWG4ARaPkKYwEZ8yrAsD4gSjx4hS%2BtV397XAx4YosUom7fL2PcitNkBLOGd3BPS5U6QLF1KmNB4fzPjVmvdrH6WnRrc1rOi0ACn2S84XC9rn6znPra%2F6Uhqj8KUKXv8B0QDdniacs6vcBgVEfpxZ4Gz3Pm20ZXUI%2FjGeEC0Mb42bEfF3fvxmCWqcCSLZXZClozmajF%2Bl5KnKyOZc%2FEJdgbFxR2ysvSfxI1mo4VtrbO9hRMCrrncN97yg89590UKAzXB5jf%2FSnyPELEGwWi4ccUa18mC7lMcGjBCF7gVzMKlHErPLHipeKGofYLiePNudCuV6RMwyh24r5rCc47mbpjaxMPj6pMcGOqUBwFt73uiYCh%2BrI5LeZxuOnTCoaqther5JdKijImB8QRWbi%2FoM9KsGGeFt7GkjIWXrseR1ZQbYRJyH1259ShrYmYLOhFaWoNaQ90KJeS9S%2Bm21kPM0TJQxl%2FYOkb6137lLEDYV0IivGZjNqokopmv1Ntq0bnZGS42OUfxVwNLxRU0IBa%2FRvyOSTI27DCrHJCfLPxUdU0l305GC3HrJPdKFpvho%2BKrk&X-Amz-Signature=cf38e428b57a8b5661bf96c76c1957f93332bf78b65cc03b4982f4c769986c0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

