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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYUSO3BX%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC2x2xO%2BmKeemTH61HUjfCxE3KHJeUysfDC%2BlWhzTme9wIgd0a%2FFqfy91QXqbCJJt5rnj8EFhAflxstSTmG2G7D4j0q%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDIndyxgXJy82ZBVm9CrcAx%2BdSrJTb%2BFSOeyoz98sXph%2BzrAWRdW2DLvglJUESuDWG0I2ped56ZNeL9GjyBsIc2lmT84JimKHpRBco5ZiZxYyt5hH34ryGDoIsJO9IqsEorHebhY%2F4PWROixrtfkledu4z%2Bj850uEDC3D0PFbVuCZAzUns4S3s6Ka7qWiGC83JkXNNT8ZR%2B9XbK8sm7yWlNMz0TDRFj7UDDZyrTvKKOuee6r7FARo1%2F5gNO5x7Vwx%2Fe5lpCzBWZr%2FqkMoxyn6CvZkv1BTsFXhin2ij9I3janKf%2FrdsNcWOvQYeY%2BM2dbkx5H11%2BMzHBUZ0YFzZAuZYnmaNVJKR4u5ZOtD8rHahxgfNEDEB5Lid9dKbeic8cUAD48i%2FMAVABhZHTuKsrsxg1LKoM%2Ft0xMIibdKB7Ca5h15fFME%2FXEgD6Lx%2Bg9t4jndxWJ5k9UfO2Oa9AOACvBiDpz%2FxXUb7NOczy0zK9xLZ9Z9NXEd%2BL4mzaIjTQclkIEsCFHUH5a9OxgeEOQRvc4KYpulheLXjygoUsSheTFFfnIBwTtoA%2BBiBcAm0e04lMvEMkwR219dyZ3meEL6FqnHH%2FXn8EHbMmLjUHE7E1B2uK3oZ5ofPy6SaGkVTPVtnlTPgPidIcOEsWTe8qEjMKCfpcgGOqUBFZXOmWlGlvXOU%2FChns6fxvSqNecXJ6czTJQdcYq5jYvOJ%2B8F2rTydAxZ3LMxL7z1rU9fhNC7KcoGH6Zq3dGVCZBbuupTRp%2BSuD5U6K35ArLkV8ozhKlRr%2FCQ9pzcBvMLPhtkbr%2BA64ivugXTjGtpjCaHlHdTTsktBr8ao5XvHhYwuXNPJG10nmP52nMfgaBVspS6La1%2BV6OxHycRtxwX4S6wyFg5&X-Amz-Signature=4e774fa4ba21001ef51339d7264642fd20eea8bcffa31407e1bda39af1e3e794&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

