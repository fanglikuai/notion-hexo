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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJLHNN4W%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiH%2BD%2FUdpcMkn5GebDH8RmXIXzSEJrEydAMX2ieZDoCgIhAPoMf8fmgo2DFrUHVHfZc%2Blvz1Ts8X4BeJ9dhOw92rowKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4puKQNROmPvpcAAAq3AOfLj8c1XY%2BRJXcudAgHSaRYE%2F%2FEjEs2ELXRoazh0TewKJw%2BEUu9sWab1LbrAW5gTLlXfYtDBS3jVLTE7ggR6kfvwNV1bDuzXynUof%2FDatLNsaN6JiEtz7ZRlCP0yvl13sAzWYBF5ND5eucO043JFhISfkSqETXxSejodJlXgU0I%2FH3bssyCoSTW79BCsx2PgRvT8Hohta4seSV4HRWuT948Z9oet87lUFWqhqW7Qj3wgtSaHtXbt%2BIuaq%2BEaCWMwpRubDdVa0qMhaCB8iHq6%2FNHFQh8pCxRM8uoG1aOb2XTvuiiTdj7pcK1%2FUlAh5VzfMOORb7iwjIBCSO02ZelswZmZnHOQXSs35QuWShXmcFl%2BaaIvzv92XIoriWeav6v44BUB%2BaD7cPk5b443wghVK4%2FZ5gVwCPGaca2C1s6GLcPljebEvTIQL%2BGjRUkYe%2F6SD7Nf43StvVdwtYBIvYIiYHGLvGMzdpDlSBCw2t1MUzWPzxd7WUPWG9wKf88zilV5fWywZ1RH9IDIWrlN5Ejdt7KFEkZFshB7mhUYPbV7HWqIPoTOpj2F32dEfHqeCca7nMDtncz3%2FMp1p%2BPNIiRybv3XutU5e1ioijSuSKdTriGo7goPT1m9USXmXUWjCCurDIBjqkAcWWVFT33Fp1ApYi9sRS2Gr6LWJfwiMXmgaNUsOY6IGusL%2Fxp1PfZSwWFat5zv5u6gMfAt1advu%2BdrSWFP0%2FqXv1MbCwkSS03%2Bvs%2BIOvnFL5oNNr%2FVUt8dzB%2BwIzyIG44TcDSYEAdY4eBiKRBAEzfosFP9cYmHnDcFcCl17FOq45xUHlJ69AAOG4%2FYzOFJdXdsr%2F9%2FArL3NG%2F6LY%2F%2BTNV7cda1nK&X-Amz-Signature=63bfa54fbb6b8769d039159701b9731ed8ae89f22bf22060b6e2aebf27f42699&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

