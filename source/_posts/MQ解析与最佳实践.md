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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7F6J5M7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCsi6MSmeeFLde0xIA6JvtPBG7PNI6K8r3f14kzsutEHwIgRejugzFGrfvTBWkb5tBwJ%2BDky%2FyBf1mwNIddIUutCMwq%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDMkPi7zLzf4Mx0b4KCrcAxPry%2FAf27DtWHk6KuuErOyVOHPDqnRU7dAWT1mjgIg11NehaBkp0bqb6jwfYmZUvwDwEb8gvME4Kz1BTXiqK0ISQbys%2FjsYiNyrY4qExZ5Wri4kY%2F5W6hnyvpSCesy9msYhPSv6100xM17HJI3rAvVXr8SkaH%2BzrFLUR%2F76TdDII8M5zzMHk1hMkEEiP1WsCN%2BESftagTQHzg0LLmbjsk%2Fs2WhaGZiOTgXrgdSEPgUJWmSptcOkbJ77X6JF140WgyrszQ6Y%2BCZiPSv8xr36ojvLAAycQxZWoGuHY6jzUZy%2BLTAXZHNCsWKdJOtgwpx5dEFIXXT6Rxdwt68BfWrhw%2BmEIeOIpEa0kbRFAbJx%2Fyl%2F0uY2aM2Bw2I04tf4fOEFTPZOt0CkALofL%2B6OFnw0O%2ByjFIOvFAhChI0Ra%2Ft32G%2FhtuwUCmlWoKN32vDEoPb600uo16gRUnbs3wIGi3gQYSgBCRwEaWaNe2eF2dz19l27hfjwa%2BmAW04dFkwbNM9s01NizQH%2Ff7q9ulBA9JKTNi18z7DYzXpWlWctLz7mzmawbqe4AO5XIOIuEs5cz3VE5aja4UgMq1wRmb8E9XDvhbsibkCrIt1JzYFxwUY1zFZvb9LIh4Jbzd8mE3iwMJj3gMkGOqUBPwAYPxtXsgyMGuQtWNnr4KxMxfpXmxCCry4yw80Xhrq5GOxohjTStT6YJS%2B8ePLvJw5vI7BBD0WrfuOjK0qf6%2FQSuhK8U7iV%2FWetmdLNJPT2WmYiO0ZR11lEw%2FvhXheI1M14nmKirNJ0so7MH5kgx3qCUc%2FMy6ZgtKmE9sMCu9bnbyjPS1OxEJwP1EyBJqZ0sub3d5Sndw1oH1z3ef8EnVrkZMtp&X-Amz-Signature=ff8612ab8e76405fa1a95b57f12367d916e5d1906beec916db213a70624ffeb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

