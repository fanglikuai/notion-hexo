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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYAJWVYD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDKiYR7CWFBuHGgCs6%2BsxdG65%2FkyYjqHY8u619o3wWPZAiAPCoBdnXQTw3uPW6gHYU61sl9%2FVASsh4HJvgV4mHPnFSr%2FAwhHEAAaDDYzNzQyMzE4MzgwNSIMu656bkVd3UzwfhffKtwDqVVW5Oz1ueFiC4Tkj4HdbARgLL4D6Fd%2F3%2F1n%2BK6RJYo0m%2FGpy89DZgCHegUk2Q9QhzSNpcJq9xHE%2Bvj7rHx0lkbXMUeJaaNW0Cu%2BgIkNUm7zoD0rc6VRh%2FMKPfJM8CxBfILHoagik18KKij8myn2kgm1ZdvrVY2S5oLBRQXPmX6HORtz3NEWmuOUHGJdz2p9zlgrhfadtddwtzHgSTOHu5ts9k%2FcoXKU%2BfT9un1zDSJdoPGA7%2BWHz%2BiEQS45iUrhnSbnPUfoeLw6yjpQ2tKUichjIknl6xf5Ecvp5vQyqKFMiC9frrql4jC1G74AAnDlm%2Bd1ezmFHmrACur98Zld7agOZ%2BhVf9DM6XajS5DyU5xhNiSteYV%2FDQvNe6qzYkJokT2fRNCH49nCdl1rr16vvOxsQsXC3Gwe%2BrrSVq5kBqnRIdR%2F3FVS4tLU5IsMj8WMGLmZAvH3KZFdy%2BTQaeHxl%2FpR4hK6IO5DEFu0PU1CG1exVr0d86hr7BanMc2oIBpWP%2BEX7jxHL%2FLkXXXz2q1TnFzMHMeZ2pDm%2BK%2B%2BoeJiN4D6hZrmVfSUnzm38Xm8E5VqPWScGlzzMEa61XU%2BH%2FA047XEwaLetMF6W%2FYhgnvb1X7FTpPKxLCKu7KRLiowiPLVyAY6pgGVQEfn744PMy%2FIz%2B9hQXHFiJCUVc31khTM3lmozhw9W2qqQJWW6IXqXYBcJqkERxF7UUzjp3ZA1gyzXml51A5qwBPIhgBYx2IadDByZNqo8jKJjaBqzH9Q2jxU46GRQlSFMV%2FS1cFqF7K5eloT7ReNCCWGUOhoT2n6meks6zRsx0cZDZnnYoRayRc%2FGMHoOcGP6TRQE2paX5fnQXB8jUbHF8EKQXZH&X-Amz-Signature=628a019358aefe5c2bd38ce16e874abf9dcbfe27ed14c823d99100c0e385aad4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

