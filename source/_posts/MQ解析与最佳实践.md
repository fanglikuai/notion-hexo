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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VM4P4LWZ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090819Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmj41ExhqSr1BgnyUIWRfqxDMG7fWB63XMpF%2BNLyV5GAiEAqnmc%2BpB5cJnbsMfMZPyNjRCT%2Bi90lTnJEwiGhOq7ij4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKyL1rvodNrB9niL2CrcA2uXjnbrsKwqyPKdW07JLM9Wpui3jZ4zUGQYohnIfpOAZsOyA%2F5yc8WrlFzhw8x99oOBcYLCNkYY0b%2FJ0Ycxvxfo1pJAVK3XBAHNX5A9vkEinJf%2BzqPgQbUL5c2CzvPOwRL6g2dNh68H%2BESlUnbKxriqpNo6MwBEecN0Yi55wOcH9kNKCbpkOWkCvB%2FWm1ln21oOy%2BWSO%2FAqmexo6%2BNB0f8p019e4V9aZOV7U1wBLhY%2BizFSxwOd7h0jdCCgE9VVNjunRb26QAIvc9J%2Fh1pNmE5%2FiCi0q1RVceT6LNXjSc9DP1omf0nvQcgUhtjJGHlHauEkODqPeFFcRURfI1LSn5FRUP1XK1CFZmidyUYo9wT6EA0F7vYZTKLvArIdSqsCvFTD5EPQ5g6WKWfynzA3cQ6jJKswYG2AkMGSEK1vXmzXvlKROV%2BAqJNmTSRJXtGJfhrS5V78ywvJisEJq8f23tPa63%2FuHkvPQwYj14FBwoSVMk5IGNcB4V4C8haWYBSdDm6GgsB15%2F4dd9CPnx64b7IKpJCctDQgYGmyiw0nhjDvsu1JY6z73M3Rm7QtmmT%2Fapar4TX%2BzVASLSOlHfAgG7FliXHc8hZR%2FLLLs6wH%2FkodHhqyN%2FSbqtkqtCVkMJGTn8YGOqUBXUm20HEiXdSKLviL6K2Pv4us3zuPuTS8hsu0lJMTnwre4gH9u0f8eQFwrw3o3DxiioXgG6uuLZSw0eiQNUSXoaX45n9l0kRHHI%2BWFWkicYxuQSNBiPjQAAWu%2B65Ml6z8v3WMKiOjBTLwKvuO7l3upok9oBaXyRAqDu863Dy3nKtKd1VRT7OnkTCjsndd3c%2BhwwbjqQ5peOS2PpPdyjoV0E8LkZEp&X-Amz-Signature=b1a0003e2d3d044e1f37625b97cd3e20a33325e0775cb0eed17b1aac7fabe2d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

