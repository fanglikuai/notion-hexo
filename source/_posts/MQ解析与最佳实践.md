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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDNGTPQ%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T180052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJHMEUCIGDKvD0g6R2kXfEB593FvjR8VLEYY4MOESLKybGHIPTDAiEA4yuezhqW%2BgITRA9PLcpmbF%2FHU0c%2FOmtBtXQgzLf9xYIqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG7uzAvn5%2BxFPjiGoircA2S0fthibS2HA2AZUw8%2BbCk2%2BeIyXMumEG%2BrEo%2BAXR4wMQE8mKc%2Fo0j9QA2GR1dU0QmwpvJhSsUL4IrqHJOX1Rd96vOK%2Bl5uS%2B3ODs9RRDVh77La2J6uZBl%2FpxEuCPpKn1a2BkdNkbKpiz4ed7daJ53ZB%2Bcm4WbVlKAFw8s1Phu%2FJ2hyBkY%2F343qo%2B8Cd9FruUrl00OgW81SEdqmhaGMUS7OwDTphcjv13gI0LFVNMFepku2V%2BlmAd5uIY8SpRtNAvHcYfNJvxc7luviqdagsSUwAP37PqKBNS%2FV2nk7XBRQte91RhoYysr2eZ50lCnl5dlXjESKAvFG5kLESBWnfgy2hfO2YGEw0YG1VimPruO%2Fi0cre6%2FvCX2lVHPXYr15YbJNa8rCgCD5LgIQxNqvX7tcR3hF2R3l5dJ7en34G%2FMndI8uwxLsMl62r9WnrnKwbQSLsMH7wDI8Ud5ZWDxRhyrXPwreBt9jHZ5oeYbr9BH%2FTTBjHcR9z4hne4ZtjWs6Z8Qg1SpQgoV1TiUSdFwRW11iPDH7HBl5T%2Bc%2B0A0SweevAAYupiNVQYkZVrsC%2Bqzrgj%2Bx%2BWYQGaVqcpDt%2F3NGT47BbdILRe9NtQWnYQhksf7BhuJsXv%2FQRgEki70ZMO%2B22ccGOqUBSJyJo3fHb0wDhqW8OCfKhk7H50wf%2FPFzz3ndEGzae4%2BEbR7AkFdOo1vcHuJimeIfi49Zd7xffLerIbQEbRuHxjgz5vlpfm7ZcBDdT%2B9DCWTwPxY9tu0Zzv24TytNNhIs7kGxTqrl3mXxfuUzXxK0QAvSEaGvyVveCtMBTAjCNlTsy%2BQmToyS8I7XK%2BTrfeAAP1QAj%2FWFfEaJFDQttMWFfNvB5%2FbC&X-Amz-Signature=8c22ffb8d063a701a7066a8e04e07855888b9744fbcfaac3729154c3177ee81b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

