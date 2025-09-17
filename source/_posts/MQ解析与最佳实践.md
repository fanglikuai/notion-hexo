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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B2KH5BW%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T110052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIDqFulw5y35m%2BKoJsQO7CHNKhtloZy41LRMeBs1jqxGRAiEAjpuPEssHCBs3TsJKDJ3odPrkdZXaBr9RzhPsYN4L9osqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOndRjt%2BcSOT%2F3VMsCrcA1Mro5cs1OPb06%2BUkRZk1jyeBzcs0dcr83S5cr5CeHhzMUxqrsLWv9Ro5VFzfrFRnIzYWa5lKgeJ7uwRSqoBbAD7nznlOUhOSWk06kmKInmZtOW0hQs5fjaIhdltP%2FOXFPRVRK72c8wgXf82xtuIj5T2VqFBtX8Om2vNG7vKC11j6Lmsro2ZTs9NV2AvNpDyDrHeNThstS1M%2FnWvBS0FIc%2FVkqinFKpljtwnoUkmc5G%2Bk7qvZLr3r5WVax99QkU0NR2eVz4FNetW63f5gqU09HReHa7SCCiEN35GNUJ1aRiOIV5BZGmcBLdQcvUutwJBD3qfmQ4njaO7YdLNzY%2F4KriL3j4KfqYGV8X7%2BPCCWZUSOScO27ONP1xLUvR3Cr0Ee3GdG7xVNQBZhVONJioIR1chXHqoJWdY3D95b6NGIHEekEjvJxyXb4zuU1wtXP3vXPumToQVJBBIrsnPcNVkh1LMk80UFw0jcpG5kZbx5PgUBfNPiAxNOGMlwMEKu9uQRcVl9T%2F%2F%2BHnRKV1eL4%2FRWVeZyPX%2BvEuNCvA4weF9QwWzojnBO7aDnQzq0hIbhF3OG29LPYF%2FrQFiyiXO8YpXISSepcmE2szzylrPn3LiRnaFY7DGSrDZSkDclBaGMNKRqsYGOqUBKqbM6qlx1jMxZytB%2F6Z0GcmkdfKzH8S9j%2BbegW3JqtRuhM34Sh3prWfFA5YB7j3qLSvncoMz6tojTEs8TAOsBN%2BJ%2B4XMOwKGwF1v%2F%2FF%2FjyDWYFrg7PojRnIa1TSo8AhjB%2Bq39IcyS%2FkTKjIoUG9lCRoYiZb6RpOOiTwBx5uziVtqY5tRoiLf2jmNS1Jr83OvFf%2B%2BiDpklSzE2XrGRGR4AwHN24hc&X-Amz-Signature=471620684711de32eb37e709fe5caec23f36ef19c8df29aa8358a44280ec353c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

