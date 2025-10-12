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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WL5H3DXQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA%2FhXD5hdse0RjU7EjmrxRyezh6RDOTteTbbLzw5Va%2BiAiA9tGCW97zPF2LnBV%2FCFtRZUkr5Ki%2FEc%2BMZoKUGZQQ%2FKSr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMM%2BXhtn6EwrDD%2BnY0KtwDTfHFb%2Fur6CbRWJRYkyuBdowrPCYD5jRagCDfChhjyEcvsISMwpdXnRzgrOp1f%2FYyJdJvpnULdoa7TXu5OKjDf3DWgoY1%2BqLwSoYjf2V%2FmHZSU6JvUjG7Np6jtwEDORomNU35qjLvxZhz05PXhwromEVZoIVhQeOBk0Cc0Ndi452%2FDH%2FD%2Bk0UYprDFLklrEJnrIYSOgnq8eitvbA385vmM%2FRExEuDOfmvmSscKMesTo9IAQn4y3lHEPQap18Dez5cl2iF2ypxIWmGjDG020Jv8696ueivMRiAYDkIjy5ywqYc3vRsNKxUSm%2FUokiTnkAGJFHib%2FhIHptBAVOLN0FLzLXogG32JY%2BdoMZTVYM%2Fb%2FnOsR1eEGtIrxOgkne98K6oNnNmrLMvJFYVeo6WP25LyOa5BBEEEKnSrMP6G7H5ogXALJEZiRhoEgqfAS3mH61yl1%2BZ3VCpMHFCnQnHcLgsc%2Fm51nF0XWCGc6rUsQYw%2BQPGTh2DMJOEJLu4UrT%2FSwToNEBx3PKO%2FRDDegv4vyIbD%2By4DM%2FXPK70wyYxKrNlJV4sWgk84AGh33xiYSwZfnhEyag%2B8XZ73%2F6zUjqJCxEFigs9MGxTr7zw%2Fi61yA2PraPTjI%2BTJ6fSD1GD6OQwneWtxwY6pgHOFi2IzJOpxEo4ftGNG0FIgEOQJhOjc%2BDCq6YZcKFMEiL3%2BlNgmPMQk7yCpKaMKQs3O%2Btj80zHGgrCQnFGohgGk3fKSiqLbpUddETuz4wZK%2FoIpUSPD0Q05FXFknA93whWLcfA8mAvsNHCfAtP0gKk5PRdF6dscsfdK5Kax5vp8fcoawjX7MNLpvJ1t3y1x2AyYJbZFsMcxMq5mGvhwKiUMd%2FOuJ7v&X-Amz-Signature=30a04f296e44ad9aa82536fe83338c58c1b01ac9fe4771cb62a16c0579f01bd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

