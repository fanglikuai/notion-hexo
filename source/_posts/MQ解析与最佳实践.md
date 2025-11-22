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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYKDYGUN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQC%2Fp%2FLXtEQ8Pn0vE4n2rAtouTePQQ8WSBYaATAlRkyIaQIhANm09tiqXWMsDSnpmITUcIo5w4uk9%2BQ9wUHcat9dEYXGKv8DCCQQABoMNjM3NDIzMTgzODA1IgytHA4dWze57D0ZlUwq3AOluFP2ZaMhfU%2BqnDF9QgzVSdgA2cH%2BILrL4boOjqBZJyuGiLFJKqjb5oscd%2BPHyVPfYs9tRgPI5AOgvjTvYFz2Tw3ZD5JPKoT9QOX%2Fqc8aEcrv8IWuILL52OesF9kamtjKO8o%2FrQdgbF1nABu3TcuBbBgoo0c9noi6Hzs0AkQvMGhx72JHdnXKOL4qQfmaowR2Itv%2BwblmjgpZgUY%2BOAuQyAAJTgGB7G8XEBxGMFsAU0UjJRPriK8gsa0dcByM3MKpBhIHdxR34lcxkzjZoxaTa1A%2Bz68RPfc0Qdv79t5IzrXZlxcZlk9Kmjof5dVe0Gm2ppLGVYnPqAMkvWXKxoqp0j0VLuRwSvvRHpFO3gI5%2FHQZth39%2BJYcMeOmWxnZ1bMZvCpm5Rw6jQiOlPWFUWUySMZKILsGWejyvogJ8kQ8uTl4A6RX9%2BzzGxYz0cq7w94ZJpC5%2BbbX8u2CzcrSC6wnfztweyvM%2FaYFs73wL9l%2FbX1AEk0kU1Y6lRSF7A10cwxHrepLnHSgyVyiVAkyApmwRdRVOQel4EeS3WU4jqtkqkTr%2BCuVeJNOt1YI9b%2BTcy1SC778ExHdIetmg%2B6oSXPDAi77q6pzFOR7sNfOhuwrcOceIssnTxy3MyjhAzCVoYbJBjqkAd6uLCA8EMYaw%2BKjKjTdMVB6%2Fu4xYr8w95LjH5oItjT0pKKrnGvvsddlFZyic799A4KSrzmffr2Tzph0OpO%2BqVHovKxJcs11t3PIKWAEqwSfG%2B2WKCnC6zqcMuR80DTTdvw6T78LwFtUmhW%2FyfibAlzLX0%2BQmLjk6CrTIJyX0BHacagtYmNEyb6v1X8PWQtiCxcaRoGKZ3YbWGdr9Ke%2FBAm0bGVY&X-Amz-Signature=869f100daeba3a9470664adebc4770bc87675a88ef43c78244176797b2f28971&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

