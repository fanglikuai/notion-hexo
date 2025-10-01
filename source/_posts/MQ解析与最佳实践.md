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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVOHTAFW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDb%2B2tzNnSr%2Fsd77aj7LLnszvbx0WeKxDDq1F7WPcR5swIgVmHpPS%2BpiVLRR2twR9lJivONIZAgO1YqvilNPOIA%2F0wq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDEB0AjdlqKlWspAhVyrcA0wXu56wSkfuvDlaJpnL4TpEnXbbGxAH7oCY%2BVfxI76XPvnWrmHxWvo1PEbsYRFhRqHXHXEdee%2B80%2FZ6ZuqYHpSOWp89ggi351D7jGnDMZ3Ji%2FfQOH9cjDOtgpcGfP4XUBrL3PBXX9aWNnACrC6eRE8EVseYDC%2B04iUpJaZ6TKiLbmM1i3s%2B0iHXEDRnQDgUbFt6N9FQMdqwJGnK5Tn0F2XRH%2FC6d9YWDFpuCjwbwQz8vSrV%2B2wt%2BiMmjeUMc6PSS2XP9F5U8Cs1jVCYjnErd75y68RFg99NQjqPhpD9ssTQF3UDIz1Ccs7eUZm4YDTwvVr3DIOaV1HtpdhfbineEw0MymXmHA2yW7t799NoUgdUM1%2FbL03SeF5yCoRGqsMVPjdfZ9UXMgnsXtMVYLTe3t4mZe5y1hCRFJVVRwzLfQADT6ADIZC81PPTNXVS6KK55PdVngm%2BoKivovz97dycZHKQDRHzy0riV3PJAPj2G4qfrmvCmKx4S1bwCJXthUWU2%2BA5eG%2Bk5COszsfBPsPoBDi9rauCBObxstJcUQE4UpRVwWRT%2BhnC25j5kBRUMbO4fw%2BEZXfGjCIcseKUnHh1m1Ksr8gO4%2FsSUvIeKhTxPURrouyXqIUKgKrsB0l%2BMIny9cYGOqUBeWZxQWuOZZKwiws4JGPoJCdxn%2FtmOty%2BeW1GjOrVmA22kVrXRSEK6Movru0PwNgN6s5BSch3uuvfR0wcJmkF3zNj8Ukflzcw4ppHaXH9r7axyIP1ymOdUsbGuuyK9Y7ckMfBoru1r6%2BMPvyff9nEAfwfZssPIbHTWj8mV%2FQhrjbd4YpnHpC%2BVEEjyQmUQyVS%2BkTuOdWS0EK8bKDAp37y8xfd12yr&X-Amz-Signature=6e1ca177430a5c72b05dc744ef0adeea3a411971f2c652c287e237adbccdaef1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

