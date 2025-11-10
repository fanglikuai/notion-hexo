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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRGLFFHX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIEYYjy0cXdW%2BEMXpvA3sJSceknnvfnQOZMG0IMOLaGIiAiEA0UAo4nkWST4iOj1zTkzT1oH28s7fazG8SuB%2B1Arh9cYq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDBQFIKQ06QxYFHTzHircA0261kYKW%2FVx5HvSddeGLu%2BHRXQRzPGgVeS6TbSIUmVSf12UklR2o9uMnGw3hNAi5z8w3g65HDFWmqbXWQh%2FbzdW4POH9R1dErXk9m9dFySLci7qTj5RoPUtO7KEQtyHqGvtXW%2BFzGYvnvLHLAH4vlpoSNwUYPry4yDFzYfe5XCrjrYDodYhBYm2988w0YuxD3vzAHWcOET9tr4qx5J0QXStzTiB9hgBOeCDEX2nd9L33j3tj9f7jMlu1NpQk%2FU%2FbBHppXfYC7eZWWiZ%2BklGa72ueik9iAgZ3uV%2FYTOV7hk3QVTJjXORRRxN0Z55BHaSZ%2BKn9AYwIEQ44MF%2Fy4YmmgXxvllpnbthHh9h23KUIk%2BO9SV9h4%2Bz9De0xa5xqfFIilePoaSMQbb3ZOaNAGhDJEr2tBSGi1uPsvcHsFrbFqKJaF9LyD%2F1X8V6PfCY6C68f3PnG0XKF8qYkHwGdD5%2B8Sc5TOZONGSIkGJyORTY59Uh%2BPg7MrsyrfRuftq2%2FmtO9AZQsDigYCLTTO4BmM0l9J0LK8u9amE6Wd%2B9RHhreTcuCbF825hbp8tCYdylUNXgT0p7Y115M6JwUG5OL52kW%2BN9Sopkq1EP6tklzGBd63j3obrPY2q9Rb4PUIluML22x8gGOqUBxlWv%2Bojxl5fwBNHSvqAIJUzzQm0zPpzpZwt5JfaxkeIrNKJnQqeSYCcX7Y76SXSUEbUbBHt3D6bdKlQsUNc7ZM8K3DaiK36tK7xUJ56lH%2FdTEdAcBCEObQsPaPAKa%2BaY0M%2FlJ9TLuYzPmIGWap3o9hTam1HuhZ03BPO8WVbrNEkSe2uBBXZjE8LDox4%2FwFhYdShK57yUTT0BX37iNp6X%2F3GDN%2Bwg&X-Amz-Signature=19b4136822a8acaf79a17b7948b9cad41cd677e3adc732b4c6b01eeffed2785b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

