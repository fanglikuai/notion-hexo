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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3Q2BTV3%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEfpmUrWYN2R59Erlecox7j0I%2BvnOnVB8J6cIc96a%2FOwAiEAp5yK9%2Bvi4s76CBgUCrGPUDRlJf68ZaQbmFBvdJLXqdwq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDHNMU5G10SUUzdDQgircA26KIPljov3DUnzubYqakhdcDqdf0ondzD%2FkYVswBA9pWt1Lh8W3PSZr6LqUbiOXmMFvoOVXnbB8iJaMrIckhMPcZlRHxrAXhXlaoWzJ%2FsA5cc0d2aswPc7ZvWPP0UfziNUdZE2E%2Fe7AUW9TAJxdCHK3siChVUvJ%2FKYWBAVymp32Izqz2ZjP3xt043uAZbNqe6m0jkkWLhNeozwlAkYJ81AdCiunWHIdzTiiu%2F%2BsBi6OwZLqMDoJ14dm8pJ5ZOI6cafZroCUKJT7%2BHqL4SoxZDAwFf%2B5pJT8Xa83IrAByjame9Y2s7YPdAf66RUdmInXUyW7LUfs22VPU0%2FLfvaDOnkTI%2BmYcEx4BwJFSA219snaShdDkLtmxiLwHPwlhv4uanE66CV16altqNYoIFcaEX4IT4JD3iBj8T8Ip9DXe9tV6AMw7UHuZWEFl5IEVshiz2Y4clDXRAyYSiM3ZVbuN6uw8fjCpf1aZBLco3xUytNz4e5BZkDcYH%2F%2FB9F871S8fecEbF4CQRm3elUFX6BqcZnszxrW8Ct1MqjV3Ko2AD61fx6FCrIMxAHpxzVkop9fBrXHmK%2BBCOacbP8V2V0UfdgHLO1ta0Lc%2BUSz8z1IApQxKRgrA9JberIg7qr9MJOy2cgGOqUBY9KQXkvNfz%2F4T54rYiJd6M9d0DSF4K%2BpeDjfjg2IPYwQftILAaOjDHL3SwsAA%2Bz0WNOx%2BPOpIt4oU5sjoUXZyb2fVraMBwpFFQNKmNRbPFeIvAiyuDE3JKcECEESq1RqUmd5LKcMHHbZpFquFl80D0sChA9YnhGfRFXS%2B44uqPK04A8%2FECPKptnJu9IiQ3H4vC9wb%2FI3GP%2F09gnRmOTg6yKLfnUg&X-Amz-Signature=00729098cd8f6746f741e90d7209f2001d6be5807d53501b6f300020b96928df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

