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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTDWRFU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHArOKYJgB8noV1fLGKTHpETAL%2F20H2ojYN4EWHlEAvGAiArgnehEaq6osthLAVr%2F1CqNiTAch3Au0NqB0PHGoI0%2Fyr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMaVTfrz%2BcuTeLUA9tKtwDhk00AmQJ2XE1TdFrebWpdbkEBZzarr0dBz7Ga3oWuqd8nVACGzkYhvkwIdJZamtP6rbWr6KnJKMgJa%2B4ccaqAwb%2BY%2FhXDE7OVyYLHCtE9S%2BgNJjohQjl3nH1Inp91zWT5vNzBUxMgMqzKDQr3gAcriuP5slEzJnyWXX5dp1fpldd9f2agb4CAnISWVE89PzupNYAPkNlufaEo7ggm0l71Zkk4M05ov%2FKlFZWbFjXcgvSZQ7vFkvRWhM1Lmm4BJuPFwGEuy0%2FMdzwyBs52cAtTRAWM9VrysLSg4iSlGBhvYQA8uaXr8fwmHBzGBuKhXiZRjHOFp9CGUfes9EKj9DJibC5HCNxp85sKUdKQhvYayXnxNrSI%2B4IUMY0sbQ5T2xkS2R11xnbYP2apJwYlvSc8zuLEizoMXIEjawg1vgrvLZFn5Jza6cxV8zHFaOV1JS2EHOWi1fY0GauMzShoIaRFrHdeXCWierX%2F5fsjdrVVjktheyi5UehVNbYuuQPMgN047TlV8%2BdEAVjZFiovu%2F52PTIb%2BO61nDqRySL%2BozxzId%2Bog6BMn%2B1TcmjFwnEbzKe9xtHpKH0Xpx%2FE48nDz7gsQ32NbTTGmvIZ%2BL1H6RCAgQuhYzhkVulZs0oGF4wnujRxgY6pgHJ0igsJ49t8Gk34WDCU82isTzOhcJ28s%2BWtjBJhn2UE%2FfEITiTwHwxSGonttFfL8zwXVe8Ukz6KP4nq6pqYN8ctQffguR0%2BV%2BU17EtCyS%2BBjggACY6eF05H0dMYUeoiiT5BGz1%2BQxnw70cORcthecDlQXYmyt%2BrZa7Nw%2FITCF0dkNojj2gF6ZpU1zPET4jpvtiOjWY9X4peh5DauWP5slkvGIVMZAk&X-Amz-Signature=6afe67c1bfea4823f4aa92f67f6011936fb1390fd9ed936df440881d61dac1bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

