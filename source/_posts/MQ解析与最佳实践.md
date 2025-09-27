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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SP256FNB%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJHMEUCIHSHD%2BLy5yHyJNMiXLiZk6rfdB2j%2FakJQtKa00qaFUzbAiEAwz5LJHRtcfou3sUJQOZPm2v26PN7Di%2BXPis4KzpaMFYqiAQIov%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNZZRZRYRIzhAbtUeyrcA5iTbrR9OvfAcsvjQDy9RvD%2FIIi7rIHS6xe3kc0VhIy4fA%2FG9AUyVKQ3q4TW%2BToqMWTdsfMhh72GWbgrfdmqTqwhBwjl%2B278iMGAZ4w6AbHvjiRo002T4S%2FIWV751EhnbJ9gGscGJ2lMdJYuFGU7agkLy5XgJTZ8trNNxKerLwy5YcAUAJ5%2BgZuBbjCi8ATmTfHN4DMsHDXKOAyMJk1VTvCJXwAyLEh5CC7TbQjUYufh8FDVwZ6RwSUcoVoMuw2J409X0Drm3r6fek5XTfZJ7wzNpQXDMGwyYaoUI8UDD%2FTQ98wwaUGG3gRqgJDNnUHqHRLpNGGSKihtHWb13FcCk1wQzcwCqzOJh3T84zLXEeFCL2hky6SBIv0gh%2BmLyhy9G31IMduk3H%2BPQ2obY%2BHk1p9C1MxG0rSVYl0q90OAM3zLBlp5kRKliwHTfT84d7x0LkxiJtOq5u%2FrII%2BcjDUz8rCGyHONgNEWV%2Fb19V%2BYT8wSZHi%2FGfMDqi%2FwY9HyJJHtx48K0g9Sfh3bTfqFtI6R2%2Bs0nDB5Wwhwy3d3tEIBl4Ry34Eub8KYJ1td3IdmS1X6s3D%2FYYxC6PfFUSNhVMUy3NmSgzaA0zFdtBVRkOymeJh3i1WtXb21MMAc2BiSMPTD3sYGOqUBiCiVVk2bXRYaqGzE%2F1Ya8a5dQyVC70XXVOK5ex33Y4W3v4IWLcrAwDljN9MQkZOTFWQEsinEuB%2FbrWryPLbuhOxZLsi4URWFVF2Ur%2BimAcifnIiVbZSqvLFwtJO4voUOXGCp3NsmyhLeZIQyuDArHmq2j95MMS081iM%2FOfk1yY8ygw3Eq59lwecuF6PcHf%2B2mV5tCp2pfwrNqheV3h4cQZWqoG%2FS&X-Amz-Signature=09923f619cdda7903bcccc9eca28978e3496efa8937c6250f7023aec9e45ff4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

