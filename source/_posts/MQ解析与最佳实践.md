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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFI2FRNY%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHeMCgyCzu4KeXcGEtA5kT6mkqN%2BXcDxkWPs9sQAI7YvAiEA9t5z1oFQjNzuecX%2B4JhY4SmbRk26nuBYKfFMc43SdvQq%2FwMIGxAAGgw2Mzc0MjMxODM4MDUiDDrDaPi3DOLfSKGUDSrcA2NRigtB4oa%2FHy14I6Kd01uOtv1nrq6zHtj5ECqUAEVkGwBbYyMN73J2Z0pQV1AVLGqbuNckIzmQuRKeaWUPyNvleJCaurPPOECqKIYchU2Ftr6uH7NsIK1IZfs753aUuqZ6QKR1ieK1TUK%2B1SKOqpGz37Oq%2F0y17ooGJMirwoJGDBC8RLCf68VMfimYys2%2FoXP5OGdXWFjWDwrBKzOPt5auA7ZJtquS%2FqZxCNnw1CIxeB4hN%2Bn4lSkFBLVNjEogC8G9xpNB%2Br8%2BQyQo7M2%2FEFMqbtV7IGH5PjhPdSpTV%2BdVDCKjeEsODOsyxT8krmCPX%2BDnyxU1UjwDK2%2B2O1U7oz2HNoScY8eKrqb4lpdPQmlFzBtDUtccS%2B1Dpyzg9EnoQPInI8yAvQBTYZGyJs06BSAvTzg8loibzM8muKGyR847QfqCLjYZNEyNR9oO04rT8Kk8KNdwy04duL8%2FJmO4oLD4CQ7k0qaOcfNBfFcukCH1pgQq1PPtL8EQiPTbHA8ecGa7THw6gsbvxdjSn4MlqHgvbf%2BgRn4EZ7ggTL9KyIfUWO%2B3wCQ5MKi6LNx2eYKCtD8haSLbQgp3hkA2fB7KkbaL2JvRVlYV7wWJGuu1wEEheHhh5WFZI8x0UQkpMMfQ9cYGOqUB5KqDkH%2BpBTyYG93c0%2ByTRih6BWiPNpt8lRhpH%2F0IlqE5raplPp5a8Eh%2FI6GRCIz6PMnalAkMckQjjVOPQVztYdWUl2QoltjYYJyTPlXhIo0jaT2Qv4XYIJ%2Biv8Ps%2Fp1BUH2rHa9O4KM4yszLyJiosJoB9iWlHMyeMi29XbXyu7XV%2BPC%2FHZ0qX1nVNni%2BUnr8906w0D1yvbV2w4HIPjUY4UOtBFub&X-Amz-Signature=d27fc52e767554cb65751ddac65cefa4f037f3ca2cdf717ce42ca84276ff90a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

