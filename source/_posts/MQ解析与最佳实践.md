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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZCMYXXK%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDH%2FQe4GIth5PkV%2Fx9z1i9wcCFqRsFjp%2FYBOPPuaPDPvAiEA4We7GRgQqBB1CumNVOr2NZHuqSyRHdinGyviK%2Bs1PC8q%2FwMIXBAAGgw2Mzc0MjMxODM4MDUiDGiDFEPKP3st61Wb0SrcAx37gbE%2FOYKsSxLlEZm6G47wyDHIcKMJvh6ehXMusTBpOhKXwYgnrQb%2B%2BbVZBGgZc%2BI%2FPuetwypoHrLqcdbSclJBJmfaeIpI%2FYBX3G185q33ty1DU%2BJnsp58wEKmAVEBDU1ubA5fX3Ju0KSknxMCp7Pvb6TbsM7rHZqSXpSSb1bOlOmYzCJgq8eeHrHnSgOo%2BNwp5BfGlt%2BDyE%2BlZB2TaO3U5Qc9lhNpki04j33wUBS0uK7vgsmKqIHG%2BIkyvNb7zblw0E0ug1ttDaKgXNomjua4ZjY2ejCiiqPt0oJ48aad6MjgnMDRCYD4WYH0VHQwi01Eq0Ha323VZQ43OGMxls2dNIig75l7%2B6JDa1bJbx8uVzco7310FEiZBxLX6gYS5z%2BO6%2BHcKiDXx3NYzJPA8LiHtzgfbeI75VsIhmREdwhtwIM1aFRiMMnQtXg7urH74aupdYRy7EheUPsmIMH83v6bfU%2BoZ26ii7GFU3k7aerTj0CZOMSvVY%2FlJMZz42QvCnKv5HD%2BOKJFRD1xcTGMkP8GiPPTodqnZC6U60f%2FaCCtLPdJhRvrz18zwROUTgVl%2B9l0uKIwLuh0LV0qI81S88UtE1Zl5Sg%2F1EZ4aZYcjcqiDB7HUm%2FSS13zGi%2FQMKLRuMcGOqUBAqndfdg7e0PpqrAJBvg7Lo%2FSmy5%2BEB7vZ7aDx%2B%2FMAJQ8RTxQws0oXwCZYZF7VcoxPpedDAFlqFopS0UXRw683%2BhOTXU0TirCnQ83xS7C1WBm1358%2BPArT9QnJ3gwDBnBzEzPNC9PK2tpam69djh8vQFrPd%2FAWHpMZqkfe1N7ct%2BMYTQeWGWQJxqXYRmOzZ0KeVKCf0I67mvdaktsC8Y6SLd91P0m&X-Amz-Signature=a01da3e619821c1fe0c4e1de147b88950e04937988860719f8be9e74566a87d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

