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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IB3C2LB%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDyrrUHtt%2FkETswBD%2FF7OntmaAuAUKAumzZeaETTm5ClAIhAKUzn6Wq%2BXNPcytZ47aBLz%2FEWrYoXwh8sfsfTAb%2FrohiKogECL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0IGdQKfSMbCuRlDcq3AO8t%2BgSa4hheSTBFgenQ39rzGhVbVJJVsCpXHvX3zO95Wdg37c48LrmsdGdax0MGiZmai63fE0Gnwr3l7Ri5tDfff96qZIqTzCFhZ2EEf2XmlvM87q3VqqtqmG%2Bf0ICtbrJ7Q1HqRFZRqznGAQG1ydKv9QmsQ7ZOrYc0iSY6GU8lEG4CY5QG70xwLhpardt0Sc8b9xTBfAK%2F9gvjAPsBQIkDuhJKqrVWwEVC6khusgNaSCTvs6pWxU6crMmBAbJRx6IiAHR%2Bw3xSJUYhE0RucL2n3GqQeSL2PGY%2BkHIDhWbqJzSJHqqgL0BlU%2FbqOYlAc22Ihsva8LyYISANSqtPpzm8oV2eauEEBNx2PoEtPDcRb%2BdGeyBVwCPN2qVPNWAaIqSEXnkm5KCiFT2B8VafmNQwMPw65SrNDg4k6V05Nxw4N7Xidg7RcSSlJif0i9%2FjhfYTui2lhBcM6t5j9x35YY6OGn7IT5Bw5ZUMeNSG1eoLrRt01FBRK4u18PRKbnBIzdSyMTi9jurnBphdLpt5Cj9m%2BvXsdSEYWHvmBSGVnCviyBmtToWs%2BNRsWoXESvl0Xz%2B2SWARbKc7qV0YlSrSfOn97F42IQXM7taTOSk45DL94G553TcSUhfDK4nsTD42pnHBjqkAdnZmbt44vWujK5f7ZcQxFUm6sp%2FV535lTLOopWxzBmdUzZ1Y2bAODfZo59FhXGNOMRdaBprv9cQ9bESoIJS2ottl1fEBh1PFEqKBYe%2FumBZAt0BsuGx8LRfvSv0g7wIF49tRZKIHhQIQQKXIw8aBaejVRG3tANpbc7nwwEJ6nmN1tHSepQkrwTqEO2JNyM8QQrL4Ac6cLiIXEY9k3ROokZ9hXDI&X-Amz-Signature=6dc451913689b46d8fb4c711b06a096660aa46057ece057995698d6f3c26b7cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

