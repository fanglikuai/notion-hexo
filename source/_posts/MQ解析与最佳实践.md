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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZJUOTN%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T000036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvQQ4o8N805BqL1GhU2LrvMd%2FPt1Jo0Ry4sMUKp%2FgD0gIgSgmWlE9fNVn%2FcgO7iz27ExB6wRjQcqektJuGlPO1fskq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FfwBggwz%2B7GgKfEyrcA29E08SnPCJYDc5SHn%2FwyBczUxRBfzst%2Bz4BC6KgTrn%2B8nR3J5CiUEy3BSJD8Fb0Z4HMIh4w0aQi7SxZNej1dYOWvVlHoYt1aGMTH2r%2FNdAKJreYI9DfkeA6IJLROF1LlzpYQGtT6%2Fln7JxXRFNi1cePDGnMG3NOeAlot9%2BcNRifEOEh1NiZJ6E0b8HSo42ygbm7zPfZUKuehRyc8WlPnjHGASJ8gvg5UoXFrGYZjUGTWNGVHsJFcf5rN7vk4US3nl4RRGcwxVbVUBX1Esw01dkZovgVcn1%2BHsGezRXKqRFnGyl%2F45CKYmoquS3Eh6o1R9IdwwwH6Tn2%2BbK8CQBoATk6DsXq4%2FG2WAUX22GYSq%2B3eGjBlfLoO3rxbBo5uJQG9hUb2oEUQ4AHaoKmeJ48lSTnybta4R50niNokL3mO0bNCWjGJJWhXmnfYYNeyXRPdVTV3%2Fu4dLuMlfuj00fwPB%2FczkRJZnJ3ogxi%2FgpfmtwcMsDoyxW5i86BjjJgDbOTolCERRlJ1A%2Bh5VTJYaaeKs9h07b3Iwb5QucbSsNetRmeh4YO8d3rO%2FDPfsbcodDoE7%2FZm5I6G2pQEDbqeiJuo9v%2FCM%2FNw3LsqM5bBm%2Bp1uFrcWz8wbqVLgCuDIgyMK%2FhhscGOqUBH0qTMoNCJLt4%2B22qfiwIaj0ZBV4B3m2VfS4BLeB3ttzrIgCYSCXV7va64H3hPX%2Baudt5URdbtLHczf%2Bma2%2B%2FaQNruPHBBrbEVx1Y62RwBi%2BDFoEIoOuv%2BiSOF%2FjkhymXfaUbGvz%2B1oGyvvLG434CKKdeNFAn9HtSVKhAyulpaNX1vAjSpwOfPpI4zs8qXgyewImxYpmdx%2FVORqy9sYHIvZAvN0iF&X-Amz-Signature=7ddb76d386533d5b2ac281fbdad79bff9b81670422d57affcc449e5d8477cd85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

