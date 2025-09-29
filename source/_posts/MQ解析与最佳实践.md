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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q7HG3LZ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCICnE8TUCpLgCXyFWh1tdvm1NJ%2BuJeQlmIl8Q4g4hhYOQAiEAuYBbdiPNbTzI%2FNykqbeBMRbClLpLOGZZj%2BgyD2x7CqMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC8WupoVncujYseEUCrcA%2B91FBdHf7Th2ZwXLz2qWD8ZGZhXBjWv%2FsR8QvyxLkdc7NnTLLEe0ogXGqHcdPlc%2BbdYDeZKd7grzbbkaa%2BkKYzO586rAWbquWHT%2FEUQ54hwm%2FSzNZfjGWSo40MIY%2FUa9WlUuDXvpZBfZvkQtFblwV%2B%2BnL%2BKDgGbXzuIOBRk5erJab%2BftVGE1O1rCvgZoAw4mDsnWSiI9e4J9bY%2FM11BGsrlmeCQF6QXM8tkga%2B075CB6Rd%2FHbbbryjUPk9DvwoIXSYrO7JAjYgOrpIuJuX6cROCTwVxI0E%2FwqIIPFFBwjUFrABjBI5e0zhlUyX74%2Foy4KUF4DJIcicCoWs%2FTEcgRqzH6VJjTdNjJSkIsa9qfnm7W6kvGtCxO73MpzFxx%2FWPjoHD%2FlSGxGkLoZ3zp4AmwjhX5fzR9%2B2joe3%2FMloq7DJX0kvMLCHFzST6TM0bjzWI2dokqjMpdtO1F0lOlr36tinbD88aGJ7kXr9jFEPu5jQTcNo1Pu1kJXTtJyJeSDSZEpn%2BVMYm7p17o38dS%2Fe%2Fr2bn1kNJNQNs6vNd7I7CC9GMQv72QcKNQxjWtOS1QIsSx7Ziit7QIXBTI1uEEw2hhF3GPUoEoSbPABiE2w1uzHTpajFMROogjVQHR5SHMK6z6sYGOqUBwSvqEAhD73NhrDZqmoejU1W6cQ5DxL%2FNgY7PyWuvRkreEv8uWRpE9KXqjaujGD6SsNy2IAEyW39BJbjIjKrEvx1bASyOjxeAZKiD7LV6DTUL%2F0COPrcxnb6YUN8j41p3l9zyKXQDF6CxZRaYvdXzJwQ9GCDTFtNV%2BX1e5f8Qar3KndZ8d16JII9xsBx37biBiyNvsG2nW%2BZzh%2BWPELdiPDPPS4Fm&X-Amz-Signature=c29ecf6d68459b734652ade1eff74ecd5dccaa9213554d0c5278398f58ebe892&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

