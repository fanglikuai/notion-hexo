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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGEQKXQ4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T040055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD1l8ukE4UhtSZHqPGy7qySa60IzYdrhMFTNIhBgGbiUgIgOo2GHR322DYZrKCWmTKyByfxpgjUB2yf4hGDM35%2Bffsq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDHSDFMLZS7ikYv0UpircA3X9cT0ibd7zvj190C11Y28Any8Vw55klJp3AuCeN0tRv9Aqh2FUa9vgJKPENfkzJ4hG81ydmWZMxZIRmeIy3d7X5F0GvmpG%2FCeRYPS67IESSqZ29PdLieAm8%2FWCY%2FqHm6BfdSkwtEA%2BKt9nY4IlkxchnCaqwDDu8RI2lnAfL0H5mrWt1KVn6Re918nruDzm4iga%2F8LOkkh3Rs2PvHrUt%2BmPdD8NbMrGh%2BH%2FqPMCSlTF4L%2BmBmGHTGqYAIt%2BFJ%2FyCUcceVRvVckD69DmtHDxAoqWQmnjdLGg6D03erc%2F1fH0gm8elB5uLsylMRuHFY%2F18%2FXNiVYRQUgEH6aoKg5HgT27c7NoiLvNh8pb6UoB6jVoFLPar2veews%2FxpBgjM1SMCI0vswckpNMpkCLcNbBN2LJMCKz3qtXv0Gi3dqQ9p%2BqF%2Bdp0IPzrdAbUvnhAXKBvARIJ%2BqyioOr6BTmGMDWvJJzrlF4QaHNbzRvScawIH0b3ZWqoZuZ4p8smKgJfs9uYka20FueIvawLj6nGGbbbQt3rKMZxyHthf93J70AYPMZaiGFOWVKCmG6W9yJkOQmZ9%2FLLGEmOAfB8bGN6974pasc7O25dCvI4lvn4EvHk0MXNC6RwRFFohvT7oFrMNuM8ccGOqUBd9ordSynTgiY1YVkWd1CjlqGTC%2Fj6nrxEEUAwl5p37d%2B1SPqHW3uvp0mwWKiu15sjcqPD%2B3B8W12iqlork3WIo5Dcf21l0ExVOGeUvYABO0BNr5IddeXM8sH8u3RX6%2Fvz1CBsh%2FO%2B7IXNwP2sUgE%2BcEGSiJbH6D7QXGeEvJ2F23almrhIQW7feucJhLV06Lz1G4PSYzib9WZA5aplVl5k7Om5OoJ&X-Amz-Signature=543cf6078a522993cae6ed1e2ed4bd634fcaf865e1374b55dd14240aff05f66f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

