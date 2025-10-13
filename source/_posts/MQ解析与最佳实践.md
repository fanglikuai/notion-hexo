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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH3CWWMW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T070114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCgP45uYdjnAiadIk%2FPmlswAgXnFoZNtRiWZSVYvzmGGAIhAP2bhXOE7HJwM67VqhJdlO4zKgd3baJ952yJc%2FRaOpw6Kv8DCEAQABoMNjM3NDIzMTgzODA1IgyiWD5bsNWL6C4Yl8cq3AMt%2BDDY%2FtbbqnsY0rM4in81PKAjfuM9DWFCCVYrIr%2FX9rig%2BlGI7jqfSTSwb0MO%2FmDL%2Bx%2FPr3AKoaoL9WVT5vO9tDqIbcwBSWDtwjwmEgHFByYq2y541lk06xHqZsXcXErKVM0uVMcPYB%2FOexdmbgoy1SvFxKbP%2BdXB%2B3Jlw%2Ftpo31arSoEQIwXM82s0F7gwScHHWRrdzWsieZY7iVRxZVOxIeXBTBw%2F4jYg1iN8vdliZO3CeR7OkpLcj1IuPTSIOYLr1SvElBwiUj5ScAVyKlM4iBg4uQaMCKv8muifVx44nZyoEOIdClMfQ0GSIcVudd1jZGGIv2D7aJ%2FEjtMUkEMd3hOZxcseQl0Qors1%2FwzttlIj04%2F23I%2FYjfksTmVo%2B03171KLwsSUW4KF7whxqfkJ8njabxdYgNULLZwiDT0Kb1%2Fo42Fz7Tna1y2d%2FWzewNdyVv%2BtCzEBkl7891HEiZWYgOYszF6CGqMONMN7pL2zB9GNtcemNagoLvBwHzdF4r%2FImfZT1BvKQxuL00cVhtpJOi5hUY%2FvkQWjrhmqCkzW4%2Fp66j8tk3yu%2Bn1J%2ByYd0rW4nGoiCvqOECKclWPoTF6EAhqlMUy97Q452i4AW8d2%2BQNVnU2%2BS5sk%2BWB0DDivbLHBjqkAZm8ldhhUkXceG0bLUrTvdm1fL6aMK5vvuR9rFj63n1hv6GcBYK4Ow3WtRkjuj8pI10vgEWD%2Fslcdp0SPvyatOKIDpZ1sQ1lq3dDqOUiZf0YfW9r1MAkYQ5iA76P0g0MZYf0U6LBHBg0FkKEEEpfl%2F9wkPFOOfcHe1AjrW%2F%2F0fumokBUMR%2BFh2sCV5EwLOc%2FsIcUqZregMjBLs5XrCE7NeCMjZoO&X-Amz-Signature=8e2e4f37e9d329ab46fd60aa7948083f369d6c1a1f4b0096dda638e17336195c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

