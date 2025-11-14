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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYASKNRL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSBk4zWeQwo8EHt%2F2Nd2EByWaV7jqQoKTKoh9eMni5fAiEAs2quoktMihVxqTGh%2FWUcYZyS2wKzDgGINjXr4TTn8Fcq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDJQydv5gtgl60OyAGSrcA6YaPbGSUlcBvliwzaeqBUoA0rztqCG7JQKvEzQU%2FKJOqnhUumoFaGcMQ5KZPugr506cmHF3qGpI4TScGgPPfK1JFi9ccxbHKericcawxboJ6Yyc46pFYhCxNWubVGHGCVoF14Lyes7L7Jl8zw3C2VVoP81YTDCEP72VmjDDdXpKCRfIxD2I3qluiFuLAh%2FQsZ2iKcAdC7v7nW699iu5Ens76eIWTn6P43iXoJnpWaFf4T3R%2Fm0O%2BGFX4wShaW%2BVyWW0SBM89i56N5EULo4Khat8kh1C1W5xa4foyRkDwn7FMbNw6qWEMlZie0q9Fh%2BIfEfhAKz6A0VQCvGMMio%2ByNe2I1aeP%2FeElUHvlt7Al4wjJfSXyHpSxx%2B%2Bko%2BFinTvXOsJyj5eQ%2F5J08eFf7DP9cEYNB3A4nHfl%2FTiyixl2OVVVCNKVzDXUErLgTAZ6VAhgV1eHI65efKhPGOU6HlULZXETK%2BR%2BbITKX6w%2BgFyYrytpu4vB6YK22ZPwppsLRGv7Cjbe4urpzRSsUcHoKDV3RBWV%2FtRuY63EHzaBAfejmyxOGtwjIMpTf5ehV9xkjnIITLOhE44%2BV36kP1k8XoxJxdV%2FXxMe7xhJRb1g%2BYBe2CZwdp7mT3R9p1oQtx8MNHc3MgGOqUBXZiREP3LoE1aK0K%2FZEK82nuGGDfdjUD%2BMR60Q3ixVcCscHt9m3fzzujCHSFYO%2Fvp4WDJDATgYsqtG1Tdw1f1BujAzY8ZX%2BI0QdQp9xLy%2Fbgbk1eHnawBeYn8DOj6CjbtX%2BfUMhUauNEFaycAOBtJq1Q6OMlOhrCGo781m6qZJqnP3DGq6zT5Nkfm3P%2B2WbcZ067tc0JRyiql%2FEygBN0zVdi4Rz4m&X-Amz-Signature=55595200a052b9f8624d61fba229dd0b246b0bd59268980ba3575ce944d9b891&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

