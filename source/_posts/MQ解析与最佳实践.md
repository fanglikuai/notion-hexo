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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YF45A4EE%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC73dMk%2BeQWblq4f9GCJCixbNPxt9h09%2BJ3ZQpz%2BunNoAiEAsnNY8vS%2BsxX4FReYziXv8%2F8KpA%2B9GsXGWM8Z4KK90Foq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDCQC6U7qHuuCTc0aoCrcA%2FCTleU87LzZxMGQn7pxR9dNN8r27ox1X9DPoQc4rBvpPysgJsqtH1AqWWgYrAc6XjpSPWTgn%2FUx3QKm%2BhxhMrcxNTW9EsyBsm4e%2BkUwjzCYph0Q0ibnWqcbCYZUw9KIQJqQgbsnzFuZG7%2BddcX3pg8OKdR5V5%2BXqyojhwO%2FXdsfoY%2FKw1mu8EM9%2BoE6i6l8kMgB8antS2b9BFlTVVAmoSuwzTYLEih14cWdjiJ2N99GtdBzeOs6mGlrVHV7ilmdPotRKDkiJ3%2F2DKaOqItf%2FAjXbU014hZUqXbZSkliLbiBU5o1jKFfuyVadRmJlSdpNvhoiRo4%2FyW6PbYRtKHctfcNW%2FrAq%2Bc4DtmdkCY%2Bi91y87tHzkBNNyVJ4%2FyzW5dD6vTAUPpEVUie1kd4vAO9Cn6ytftOe5By8XvXAmWQ15iMsVe6TAsK2G%2FLf%2FMTkGb0JbaSPAJLDjH2khh19TNIPOVTahh%2FcNJVRC%2FO4s7nT3Fgz6BaS9hEYa%2Bsv7jaabjB7%2F4lZ9Fz0CWzDTBeMWpNnVKPOdgVKyca5lF3H%2BU9MXTlXzdQfZg3hUnR%2Byxgm%2FRDeXntHLtcfvdKBiMpfkSrBU4ih9Rc9N2MTnjnxz99lAdOD22T%2BzUDcPFWG%2FSYMKiYuscGOqUBdVHJA1r%2BZDkNhdOiFdF84ipTCk22YK6VVi%2F4%2FZ4sIYGpc53dFZjL%2Fk2uyS9H7y874qwbMgrM6H7sHWZzs5R8TO6QnD6zvnzfu6uYhyMjwPfySHvyGFd4PuAMFgblyT%2BJ5%2BjnRBIjEmT2Xwjc54SS6AhPGg%2BLNEDsfggAXoiJE7Dev%2FHmmMlNlB%2FhPN%2FoFaBavv5eWBPtuj8%2F%2FDTs5iFcLRQMgvSl&X-Amz-Signature=4ff70fcd02419def46794414fbc0b3f42727c5c627549bbba0e1618113f127b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

