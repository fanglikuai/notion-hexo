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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSAMFORD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQCscccTv0PNHtRJsQ0fks88RVQ3hlw2olhrgbxKGWIHEAIhAPjntWvIumRf43dj2GdNFaBxpiJegaoynsgg%2FKaUj5pVKv8DCDEQABoMNjM3NDIzMTgzODA1IgzjKRU6K8b3zW4KdDoq3AO58K207u%2F%2FAfLLcciIt9cqVHsHoZb%2BD7xUbpsi1jd%2F4jgG9SJsPuFa9RzJnzV7xbwHFl9PyDKeKmdVtKDRlD7iB323x7hPiPsDW6Ou3nD0dFqaT%2Bdx%2Bxrch84j4cvNwrE%2BYGMMhWQNCxKrm3fiSYo5Mk6wR0oAGeI5%2FESBRsR5jDNGAuIU6TIfN2oAM5U2BudeoRshAzpTUdOf1GysPhl9BlFCZwQztkrqBUgEJRwN78a7Jr66VPfHa1qBlKYfzc%2F9ELueys%2F7kLUGlgRMI1Y7gJEubEuKl6FCasLJ0Wj8BV4iEjYKtB02cGHcoP5Kup3P0Vipho3dzQklWtL9aTR3NzQ8Sd8pRzx786EMQ29QdqRw7p2dDQpNM7NTizdxMMCSEJ8r7m%2B9cY0zY2FCnK36VRQg7RyRF4lz7cHkokstHqypwAEPUDsNqjeZbkIEKFZ5YJ3G%2Brgd65OFKeHss93OvzV6IZ3iOzoh%2F%2FVaDpUwHv9o4NJ%2F9Dlpnkz8kBlzU5%2BIJDN9PTD0nvLaCtY%2FfVQb%2B5A7HN%2FUEGONyI6dDoZCKBW4aHnspXoXEi1SxmaSWWjDrH9fyqZZ2RItdtMDPMGgKetOxx2tVTaZLoa%2BrFIvax5ru6NykJlqRwrf7zDHn4nJBjqkAah9NYcY%2FMhv4dhJ8T3F4%2Bi8l1ZWQe7%2BrHULgqrBBQyGJMgGZMTTth0MAhZVTOF3%2FbnpSWnMu%2BkMe0HO8ZB%2BxkXahkF6csElOOK5BcIrXKRzh5Y6IG6zfVOWt4%2FK6Bv%2FeVII3WNN6e7UCBwNfQY3fyzW3iYZroEKcL1TqpTVCYSr5Rawe0%2FR7uMkTI%2Fk4dBP3Jqua0E%2BDIJmyrDatF8Z2to0WFj%2B&X-Amz-Signature=29f6588e270c1dc1aa00c4892ae74f62002fd93fe99235e1ee35b7b0ab6328ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

