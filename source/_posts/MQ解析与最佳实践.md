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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THIVCJ4D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtclKrU8SvaMU9M00jjzFOXRpFudG1F8OgrAs04iy83QIhAMOFLCYPh2Z%2FmAn%2BY6lssMPQcTNgs7SpyrXPRIUpfK89Kv8DCHUQABoMNjM3NDIzMTgzODA1IgzrUzLzUAK6at6yK5Qq3AOPVCSiXmhEBCSzA4v8m%2BFrSYXJpgtgSc%2BDKkXP6BMD1bkw6h4XSQhdys%2Bzx1Z8JEDbVcMoWLtaWPtEgg3NDU4M1ajxtROocCLnf6%2Fl8fA7CayDheXE1cqy3FWI%2B6gHOESZMWrniV2JRRpRjVCLOf%2B%2B1X5OftHrYP%2FECHd9O7ZYbNJMq1FFt2EWzGWIHM2nXtc9eBXdAH9gqY3ySGZJZrm5nKZk4OtjLGilU18dAJlTyZLUC2FBdhv6Q7IQ1bYL7c4LuW5pdSoEFyDbPIcEAGc3dZiEM5%2BQR7IuNVeSrPtkkHihbMdTtN%2B3hUUAH74q%2FQyj%2FrEzAB2mn4krPu5nxRS4JylSb64Xb783q%2BLTBE0DIFKnGBeadNeZ8zuUAeyEqLswXmCEDEksagVSIRLqv5T032jUSgYqbrgK6I53mxgsueEGtcNf8hibcJc1weKnMCUqZASooekjgCjWjKRVbxKE1CZSeJ4xHEs%2B%2BDTgZ7DiRqVLupaJHLJhFHVP2YMe1uz0R3ZemOrNR6%2BqoG%2FYHHufqvBoWkK8%2FKNqHa1DrAm5UHg9Ve0qp0vlDFBLvEqWobDPAeUd3T0X6elSQBiOLpEPREFJRU%2Fp9gAz4bE6JIEM06qiA9nNxUJURwQCGDCT0afIBjqkAaUGP%2FkdKT9TwDvo8ZG2hCiZRgfM1tVlep1u4093WPOFi6RlOvqBdYtXA4k1Gbmio4Z53JymAV0JJPJRM3VsSxIdH2FuMS58yarMpqK%2FIRosDNwd%2BiCC%2BgbOnDKocqCYG%2FXSnqek%2BqXd3aOrEv1uZZvYwLhHK5MsWj8G%2Blx7Ad%2BLxKrJFoI0ggEnV7SmiEJFJ7AajexMX3PBIB0xtZj0o8vS0NGb&X-Amz-Signature=9750cb25193ee2f4c2fc2014397f88456befd6ce75d0f0400c1f9ccd43411565&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

