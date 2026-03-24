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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVAJBQHJ%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T125107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRRESIxO7PQJFnAEgCj9jaBiN5kb5p0Fm4%2BXIr8Xyv8gIhAKcnIFF6iAqseeUtOLoz0KyY5QajkY8aQqfj4P7%2FlxYzKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzkfEyD04%2B3fnm3qEAq3AMOlSUsYuV8UD7gOU6XQfL3ilkdvcr1U4O0BewG8RTUqf2kp%2BrM9sZbAv0ISMuYexNtZRJi7Ffh8YLyyIZs%2F3JWgLmO307XZlt2DGmfAV3wDcIYsn3ymy427sW1fwdUGzUU%2Fi9VSw3vTb%2FAi8aypbwJp7uKOSaxp1mDgjr18QA9ASKpXYgPMfIY2u2VZvhh6%2BvdQsGAXDazQs5jH1VBjxPyLqo4dugTELNJpz8E1ecFK1XLRnUz8sE4Ink6nYWBtthsMHPYX7%2BsOjJuoZgLQ9IjEZGfGtoCKS83mooH30TfQJobLB110h%2Bv0yxjikJKanY0eI%2BupnpMIMBMpOo1ldRbZKlVA1TgAQbV1GhklZ7gwaKiSkfHN6RBVrNHsG%2BmHVOjgrnWAGqlJYalapMxo%2Fwp81JaUv6OohIkQNSIx0Ggb1xspI9oIQmigUcnnsvP%2FdedJF3yo89egC5xj%2BHrOUB4QbwH0IdCAG2mam8S6IgU3QNEqtp6NexkC2lf3IR33aVZ9Vy5LiDNQ4OwYV6uog9CAcBsXCmpImo290c78UubJlZLmxnqlD33QysCi%2F45B96Td%2BB1%2FeuG4GHszqqDKvnoJF2jXXkK%2B6qMbCKjolj29Uq%2BldcJKcM%2FZ7%2F9WzCG8onOBjqkAWsVoK1AnJaLPCxmXmHVt49JUF9aUdgFguxK1s19CipEE9nQn2t0uKWIZJsSEm3wHUEBvUNm%2F3zGVZzJ%2FsIB8mpt1weB4qwRCzIP%2BacFtO%2Fk7v%2Fbx%2FJXku%2FMoa4IVo5P7bD1cZYRFNTAZz6%2Ff6D0fLG6xWcgXK8b%2F5wAhoCxUPYnmiNueH%2BNn%2FfMjYZ3Mh%2B7e4mhsxG9AzN7l9ROJ32h0NQCUGfr&X-Amz-Signature=b484bae1e31eb2d6235d1844f87f33a9d81c8746773ecfdb566a60c817349c7e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

