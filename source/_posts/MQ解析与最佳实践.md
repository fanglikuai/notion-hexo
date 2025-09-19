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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SV2TJGXB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCePKFNWJcCH7eTXLSYnkvLuLlJomyJtbVUEDCw7NX%2BhwIgdi8PdDsJBRsDVUrH3fBHsarkpZuptPZz3EYZ1F3AKlsqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBcqcEUAze6JLkdo%2ByrcA%2BqU4bJOcSfHfxosOLfevMmfN9Z542CliQoqFDtImCWwC8OTDHiLKnIvMY6yn808xpZJFCs7jD2LuiBAN4FDO1fyrDq6uDcyuZNBhWEgwdM0mqMh8wLFbeWyYxeKHUNhbIPFdz6fj99MoGfIlKBkkXimmzflK6eP1ka0n7iJfNIrSz2qImS0hHx%2B4W8YIK4gSpP124JncMZNmAnwcpLeyFR9FCJbCz11ZO2g4Ui2LLZMBvpsw6RGk5rhe9qG3KvCC6CapxjPQYAURbnLX7F9DsLHTliJHmrnQRSGRJDdnxcEi%2BEwjrIAaa7rAHTOjhbcS5bty0%2FTRtECIRWKF%2BWUw4yxnRJp%2F5MIXUGKAKr8iJZ4CzBQQEN4b%2FPQsIiugKWUUhdasTMjGqI7hkBaeLzWlrKVVrYU%2FMX7eC9xMFd8Dl8PpwGWvuIACdwM%2BL0cEoHPFoeIg0ss6YB5GNDUI%2BVVVxlVQbiGzHht4SGsX4WyyittnhE1%2FbHSW5xK7dGvZ0J8xUGiEHb0uI%2FkOznvmMqPpq%2B8bpQMoktam199IrhjVSNE32gt4yMiXJQ8MgqpBZDUMOCJNEHHb%2BrRibTINrkJyyurAFq%2FJUFj041bgoPqXrA8pzPFWyQ%2B7o2NyxYgMJDjtsYGOqUBiK8jtQOFWhIwm8yPK3KzyfoGI%2FdiAhCRZcoovF6vYvJloApEp5Sw181eDNt0WCDl5vw9a%2FqtUkEGIS9jckDVXWvvY%2BP%2FwHZRonOwV%2BVTnnIHhzWkAc%2F%2FHbfHkTMmAUfP397nqfAUTcaKRB4mffbVijQbpAdaH56eOUXZowUhFaC3lhIVDY2r2jbphSoA6qvdYpZ4Ab5ssU8obTwiAtY6mmFU3GZw&X-Amz-Signature=f625319ac7073fcb63adcc2667acab5d011c66faaeabd8c557c2501a1f1107a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

