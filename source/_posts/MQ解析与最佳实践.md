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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ZGQZSL7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T160230Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCICw1T36siA8K4lt3BJcykH6pQIoDcm%2FUcPfwpUDEwL3IAiEAiymgYdy6yw4KGoOcc%2FRjdDCoyK%2BKevVu4SiBoUk1N9wq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDMOo1M76eHi00Kw2xSrcAx035vNtg0wOFwoVn2vXgyIYPfG1U7HSnUhyyIVYFaMsTK%2BeVC2dZqnDtG3WHbLos6rgkQcqw1u4suyZ5QVKhAattGFRriRMsY6bLl9KqbdUumZnGWlZpYQERqYzMerjeP48mt7j07B7tPBU0WNsJZWBwNG7PUhxxFJ%2FsGI05y3lt5xP2WccyiBGOHMDm6aoKkdqexC5yLtnnCP0Z%2FDVdMQKBcCpiKXIL62pnHiKSfpeoeZErFQwKfOpcZwXZ8sWqCcGdYONPkjhUB9c%2FaZhjmfcRlctnAUh%2BMHfsbol%2BfvQCOyUz3iJw6jFEa%2BQgA8n2y4wpOkbVIQx6vzfOvL47v4wQZm9MdZyRsw99809N1FO74Ig%2FsCF%2F1Q1C%2BT336oJx%2BxT7dCQ1IPrLFsferzQN3NLR%2FcpKposjff%2F5FKp47tmGyq7RKyvLCwMQb5dyqV7WQAIz7Pe0%2FPH%2FI0q0TFYGEyxHurYkeYWgUeRJlZzC2s9pMErg6CtWTaOe%2B2ZyAnYXqP8ielLy6C0oC%2FSZS2aUt6MxNLuT2kwCzdg9cXs5Cc2e2aIdHzNbjJwjihYHYkjQSer3RNKHtHG5Fn8K90KxQqaaOfRMVjQfzx9wtM6yhmqrMcsZ7eBqXJSKcrfMJi6k8gGOqUB9xVOebYY4MN7sRXly5fPHaUuBk4NX2yfRihDMMH7J236ogfttE87RDf7AwF2DLWWEDmhCEVseH7I2Vv86QSVbRHixV%2FghestT0LPT7YsKHvr90Ag%2BwIxVMaa0k%2FdZXMGGc7sUWN%2FLwM8p0778Eu0V3L4q7nbLLuxWdnIqAzC%2FsZNqTSNpZgqLVmowY3I2LoGLVz%2FDl1hZvt5IcVOhzD9ceMycsKm&X-Amz-Signature=8b0eb3b6472dc3bb7e023e3a9cef72cc761bc531b29189268566909db41a99ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

