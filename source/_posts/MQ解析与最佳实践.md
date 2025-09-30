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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622JHCDKA%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T020517Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJHMEUCIH61Y%2BNPj8zuXptWap9WVHLlvQCwqIZmP6Ab0tSFcbLGAiEAp%2BW1ge55KBQOxvDtJUZU9P%2BrS6mt515QsN7%2FAiw6NRcqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDPIcbxDhnus6ZYafCrcA60d7UjZ5WgRGElV1tobTkFD1B8LH38vSTDwnv6h6dsB5Cy%2FFC4umySoSJw4UJZGFQO9xlAw5s8fdToBabZUyyH6jSxyoqxadQBkwxKU4XZL%2BzAe2eZz03R3VuxibX%2FpRQaDJBRFRHjln7pNlfC5IkqCnb5Lv9FWAyDhTbZozXL6ASBOyAVuZVsqQFSr5DrDRqydk9KBe%2FuoIQFHpRSXjxdTNg1WM3BAPvynjdQpUNoc2xj0LHnvTLCk1Hdc0L3Mf54EqdFvqn%2BCyjgqWWeYPVhkuwiOrDNQSnTT%2Fwgiqmhgy5jicELlINVRGBI%2FymreXL1eHT0JS1mwHgfiOuNwviShozgKwAdEXwdmr1VLbY1HqwEUKGqu0v32j9FwTE54I%2FNaWmy7REV7cwfjUwy1tTooDWYmoHEwAWG26qoeR6b3I%2BZk107%2FYOgr6%2FcYq22zeOPI1Thab8cpq01GUDel9nQ7d2n17xlDC6qK91M4%2B4o2dSWD1%2F8dHlPllBz%2F5n374BH4p3%2B3SGcCAohnhaB8wW%2FC%2FDaHvTLCfsT%2BQg%2FCnWpK4LeGOVE6mCnE80Yuq%2B5gJtZB%2BpxKTDT1vXM49kWLtK931xWxDr7nuvitdYJ6sb41buZQSjRvua8ysrVNMOrn7MYGOqUBPV2LMu6qzl9LLvx6VVLtzwtY3lfayPm%2FJmJ%2FfrHJOv%2FvPKxJdu7JqHlp5XQlCse5IdJjq5NLDXwtU4sSosCKqB7l8OGMQGI9Ah9JjBUvny1YJGQb%2Fq6NbddXtYf69AmN4qJF3LHmZcU7PkgfV1%2Bbf5ZHQGAB%2BLdfpo7i8jXtdycfthuuxXtWDIVN%2Bal%2F8Pl2GewdG58n5mlZlsgDbZrRp4cp1hA2&X-Amz-Signature=a2e9dd52a06baf12090729f6f452eaef753a6d154ccde679a89c3c2c4ba3472d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

