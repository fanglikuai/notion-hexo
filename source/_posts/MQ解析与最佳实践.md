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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IERB3GO%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpdIf7WDkPueUa5R9wkhtQbO6xZbUJYRtY1Xr9ORRrzAiEAl0lSuXKKiTrti4YOqOOLqDUOOVy9w1LU1lmAVRg3PiIq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDBs4T52LSD6ExtL0xyrcA7mKA%2FNk%2F%2Bt4PNHQnx6Qye0m1%2FykXdfmE1Ouuo8AiJSghA%2F4AtGhrc62YWtSdK%2Fx8wGqjd%2B3%2F%2BJLARTIiXZfoaa9XPaYXYZvpxx4KFlIXigMviwLYZ1foVnsqqQsgHrGiELw0vaMjUWpZ39t5nBTRru0Tx28%2FFwpdD8eXVtHjbFkNnFr2c41v9R8Boq9K7CUy7bkGE75Epvpufb3ndP3Y7q57tjlgBpDSecXmlHs0UXxmsPRfPETfCKav4uVSv3sLTM3Wy4Qb2s6PDaaGBuMUpHdiDNk%2Bz%2B%2B8kwfQ9US8%2FvzfOeMsFyBPzadCd3iMVEtlpXOvAn6VNwEuOmKQ%2B4h%2FzzT82DoWhw5ILlFfyfMnW4Wr4CamGwiusFT3xwfGRLZxbedlJPakZWsbhwQwbFyD96ztQJqAXtKIaP%2FlNjx7ta292VpcJGhrfbpB3044HIGX1nPRiiCAtI4BXUYSRWDmn4TFZPx1P8aigxNSg77%2BChVeQKGXZBhHrM%2FtwOasj069W1bMgYhWfKpbeL9ldSfaqILH1UCFeId9vYXgDSdtOnnHWXROsIgVv0eAii8zEFEzfqT5zXlppcbR5H54vHLaxVatPx9vkRUVk0%2F3GyXttSXJm4AalupJXPzZoA9MOLDosgGOqUBEBKB3TPAed%2B1wXvYVNqw8lyoxkVBHHFii%2BOgQJHXmbsVTRn0iounXroUV510MngqplWfqs5jPd8%2F766Q5HVSFZp0Tsyjp3OL1%2BC0aNfUslzyMgJg%2BdFZ2GC8LUibsblnHelfx0jzi8%2FHUNvJoc%2BivzZedAiEiLVQzxci0%2FJ%2BBbvV4irByoLUFdiq5seSp09IxMQMcqJrCW0WUBsD3GNrvi%2BEqgfH&X-Amz-Signature=9ef9d5751f120575f03ed04e6c2bc1225d78ba79bc59131d32f298bf8250b465&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

