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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SS2PELF%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFpCR17vbz0vdMdIL3At9kynbkpwrpVAys%2FEAyMPvMVyAiEA62LRU6B%2FZt0y1GHKEqpw7isnvW333ZLGikbmBxyt2CEq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDFQl%2BtfJIiE%2BA8A0eCrcAzobhb174tmYWNXLCppp5wihY0AvzN0NzvQeg%2BXQyVHhjjUjEE8sWpffGBaVRpRs%2FyP60WUQM0ynKIE%2BRH9c7Y4h4YeEkX3%2BMnRFh7prERX8f72vrcLYOyPZi5y%2BbccqKihiD46rVqZ%2F8jtxte4%2BrMuC%2FU8hPwP%2FJM3MIEHtaa6BMbKCIN5yw3QZ9d4RrJ8NiXb9NWPEBjYZ3BxMrlRQ8QIn9T2r9IfB4SCaX9wjfYufTMwJxYMGTLyOvjE1TZ4Q5jjqm6Tfj7l%2FTi3iO%2F4WVEHNaLYsy832XhvwjUVvIcz98JJgwSq71XbmUMBiVFO0DXos1uTmQb%2FsUpPGKGUP8kM5GHoUsE2M%2Bk58SqPnm3%2BBPgKyoezG6dmMVqYyv7FQMofM9JkGwfEbGYITdGpRq%2FVDd5ENyhr4Dyuyjkvy0gBMdAuxUIuBWCmTn%2BZl3S9uEjSP3ligvyg7FJMDvYFUiLVc%2FuhQX7yKvbkcaVTFh27e66P5mJVZPafVqSL0%2Bi%2BntyeUvDUu%2BvTAlwVEb0KJy4TeZAvmnQVSmXbhFsZdF4%2FNTPdLN4abz4CWhvNKy38Ph6ayqQpdKszyXQT3LBz52BZ07XrgXIAu3c%2BeJIYGVYSm8I%2FZse7JgcTxgN7mMOiyyMYGOqUBtuviNzB6Js5Ijbrpt5FD%2BwyzqvEDUxS3HoKS66vH%2Bg3z2iJsmeuJS17ud90JalX8lCrYnRoyMSCtreVE1LkFw5oymI8rNS9B9hSf%2BTI4NBd%2Fqknha%2FkTtyiaeZWbTlJGZLQmA8OcSDCcwxNEOT%2F9BZShNoHntfoBw5LHP%2BPaz9DE9VHRV%2B%2BFLFsDRpuad5yZmIiSTHNe8PYG1%2FS%2B72rGmNNLGVPn&X-Amz-Signature=88f94ae6ed7e85126b13a784726e230bc4f3cebf602212bdbd2a8fba2a629b9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

