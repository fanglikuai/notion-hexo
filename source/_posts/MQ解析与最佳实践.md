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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIXNGJOJ%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIGl869kOcyKauzSHBAML7ox%2BcZnU6Ovu39gedKUUzftAAiEAhw3SNnuyDr0YCfpp8pP9fiJGsfqDTAZZn4QZjhBzC%2B0qiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBRR8Bxz%2Bjbak4L65SrcAy4A7KjWYn685bc6DgEjAXT9uesFHwLW8izqdFbE1FeZ64K51bjScukRzXx%2F59%2BgW9x9NzPB9y7ZSWC87449q%2FFD64Q2dxu9WjK8LVL0NxLsnWmc8cVa4uX1oKwHcYwDO1NyMevyCnzkeF0OFqOpL0%2F385bT7N6x%2FKdWpTiGo77FzToiYIbgpMtO151mthb03FmsbBOA5SeGrjUcxMIvlYaB2DQ0RgdkibWqs4%2FBeMZkCGCadLyAPhrlFUV5SDWwXykwuGw7eRlqBdibPbzSxrCchzoGjpqzMafbVulmuJwVO7xAX7xdLjCTqYZ3GCmW3GRcnxD6K1p7ae6xbFFiGfDG7qZmbMbSnGuJBZVIFk5imtfeQrdGqddnBQBtA7eR%2B7Po4u9%2B832OJXzd81IYORM39Uvv1qLQutDnmkCSBmRRPO2iuFwMNpvLsK8needuL3MFHHIJETE7tEoXKE2DR7LgV2pRNV8cpEO4Ios8QJK0%2F5N8bun3077vb6FSjKflF%2FCBXtPVYZZtM1W9AESrWLYihDWaFcTNBVxJbru3b%2FCoIE51t8wd8VQIeYRn9SJ7Z62q%2FBFN7pbh5N7h%2F9f79xWFHOcVa69mJnxJw2GjmMs40PYzp5oclvAWqftwMIOUvMYGOqUB4n8YSOX%2Fa%2FQ8468VlFwSehCA%2FZWwrnDLpFxIPB%2Bb2Ur5%2FrDG8kRrgC638f67kMfYKIhvFMg7XL%2BMxrfCdY%2BROIKBKWt1DOsfRUCnjiwzq3nPvHgd%2F8xMtjqlW0H31VLOfS431ABENKLueRI9Y6hzM574oyfXRTnE5tG9fochUd3AUtS7svVs9MUEX66L11vDkPCzZb7bTaiF2OIbbPsprc6Clc6b&X-Amz-Signature=0ff8b5d1d5b43a3abc0e74e556684b6ba06b5d6443b93bfd47bc6c4fa2fb0505&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

