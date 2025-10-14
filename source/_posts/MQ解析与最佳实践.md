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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632HGTDNZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEa8h42jBlglnvtPz6VfkFSI%2FQ2rOFwLPn0Pd3YJMeF2AiAkBM3kCGsSthckv7kaHL2A9gq5kZ%2BQFfQL7W2tJ7cvLyr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIM3TOJrFMTV48ODYGCKtwDqelqNSP07%2BmAT0GCFrpPbJwTIfhoaqu8NA%2BVsVDyVgbIcCbMPFpbv3LRXSX2NII6cZmYeNSIFW8x6DvK6XWKMnS1zZYUGjDy4wUF1%2Bh%2BC30q2wPgLBk7PxOCm9t%2Fo2I6i6cX%2BEWIpa7JDCG96nAIb%2Br3vYHH9YSB9xPeh%2FglflDNFl9redkm5Znj86DLRBivrVwfcQ5GuMWdfuFRWZW3kS41JPP11yD4dtxCYXVglBbSbUXz2E0QO584nQnqNVS5N%2BX3O6C8tVMaYxCGGdb4YH02a9u7Urrze%2Faaf6wP01vN88jpaBwrmj%2FzUIDK%2BgqBxmfvrtH2R7cMt9%2BEnOj1tqF5YRe2673Cnnhu0PM53i73tAYnOUn2CGBryhO3dR2Cq%2BJW66Os5MC8Q6H%2BFARxCxl4j3VBlyX8WBdtd5Hih6JKle1oFga4DX5TyvQvJWQhKSRaooADF7tNQN6ycLxCbxsYT%2F%2FJdBXSVw7SvTmj94qnKwakjYGto1EFJvwoLooqLdWajdXmJ5V4GdXM3kEWs1JzSbtf12pEwTLSWzDsf76ViNpZDqTqngKqTM21UTWZCB9FUuoGSYyNrFRbd21WwS7g4eGNaba4rVI3S7E1EM1JLUXdQu%2BHh%2FOD2Pgw%2FsK6xwY6pgEDgFe177UUxuM6Ap5GhHnxPmyM5YyS0V41HHwlsIn4Z%2BhGMbpowhz1iHwy3xyy86AFXtFmuZ8OEW0FoQsrI7gT1uOfnL7jHfn21k6sK3Jb53kDVqOu6v%2BqzXHkjMgCokwTLR1oMjYG0RSFyVxn%2FdWEguyY%2BbgUjMGWTQz7rwE%2BXYTAnD0XaXwIUv0dKRQzLMKUukG8uc78kKdFRyDNM6Djgm7z3%2BJm&X-Amz-Signature=401df92fb5acc788ab748905f0e407ee2ed131be25a5a7af4acd3c331118cc1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

