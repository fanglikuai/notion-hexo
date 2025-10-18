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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677TP7FRA%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCeTAJJVaWv9h82G4yajd8YvJqDLvHwI0XFgt7b5xMViQIgLE03DC1kIxOzZUtMdXx8TPZXEf4ucEUpR%2F4qb9%2BHivcqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAasM4N1vbB1oBAObSrcA4hrL56pcxh1QrtESWycjDAE478yEwD8MYWUIFCyavjvB3825gH5Z4H%2FKRiMiKXvyRU3nhb4BmtbGR77ujzWV5mrS5iVkQYgGKdNGLz3jrCSU80WGSm0d2yGjvX4Bf5rFb%2F%2FFl6F3MumuuPBB7kY9sGla9YyGd%2BJshvvEKGJakfiThChehyzAwfDL5STNoFHtHq0odqYF1HWPLp%2B4n%2BZM5mbLQWxJPls2Wn4%2FWPEE5S9g4S7Xo28hxG0a5dPe7UYfAM%2F3dPJ%2FCt9Vl%2B7WByWMs9fHJiVS7zS8NK0WDfONVNJLZlntTHCFyiHGOlT8gFGZZPeodaHzC1LAv45txolKh4j1NtugwnDPYP3E2OqMO%2F20t8pM8ED5gtTveJMJExHquhL8TPLVzXRcUoFmEifnEouBEHoJK6e40Y7Qu7kGboqxr%2BVJ8YewHcRRtMD0CHIjQOtED85mw7ordckHyv%2FjNTZWucZQ%2FhqFhCNCMHMpe%2FSyPr9YwsHAZQWZDGv6HIDLd4dyjBa3TuAYmdb1egGWwWTwKsKOuFVfhniXrDw9X2FW48hN9f6lUP4v6hWLOcQPUFKDnFSEkI5rd%2FbvN7sCXVsUFcYFc%2FAwSyfTt4E5ByM%2FF6Y5miazc74YaE%2FMOjkzMcGOqUBCpX2E9113GTMlaATD5QflAXLU6a8Guy5d6mH4B8RjiKvl9kizc%2BWU2bVBxvUGZkEIKlGVowoMdRtToI7UERgOr3K4lRmEk%2FCwyXdtjBrfbF1oIhI0NNZXK7Z9LhmSztd%2BqIdNLzXLjIN0r2ZCnx0ic7Wi4EtMsx%2Bj0Ii7QRsgz%2Byl3qirpm9Hhht6qY4Sos487fAeu66hLyuTE%2FvkqYiVpcXV8Rq&X-Amz-Signature=f15c1a5cbcf88f983c78f0ecb672924fe3b9c8bb803b9d0420fd80392937e78d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

