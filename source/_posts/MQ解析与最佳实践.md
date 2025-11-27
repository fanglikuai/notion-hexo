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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQ74EODU%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGarWnwl1D8mdclh77jFfWHm2%2FNmdGirUzAworzc5KCUAiEAu47XdEyQ5yP4QTHUsiri8IErgXwa8%2BojrnP0oc8YjpYqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJkbRHKbSUz3UDFaiyrcA0Pf6Ou12Wt4SnwFkInr4hEatsRsKZV4w3yvU07TiffQebKeB%2Fdpch3hNMutT6EBm%2Bi03DLCsTd71youRMpcYPuCTenn3h%2BNRljig0SCLFbmI8DhO824SdNhDv9ZKJizOTwNhibv1s4Z2SqjJ8K1cKnu7HUVvaUqJzVp6x7oCdK1YX5RZL9QGlayAs54T4kmspZIE2SDGZwaFdiaOdZbi%2FDkLq87GN2cdID2%2F%2FCBUOXy8tM%2FMphh3xap2U7lKSm6a4AI%2FPXsFENLEGbXaZDW9k3iUqOEa65O9ZyO8Cka%2B2Zd6N1euD3xgj7TN1VJJChobK1jHXx%2BzQBKsdPe1pGPfN0jr4QLmd3WpL6RYeuqK1YS4rrqUylGQLRJ9cdmTWe3sonYgOHkXTeVWnkYGtw0eK7zq2RHrCXSW7wBAN%2Bxom%2FG4ePmJnTLyikRjRH9TQlVyQ8757dFAnB4DzUU9XPq60%2F%2FXhXHSX57%2FAT9MPIlG3pgz22d4r1qw6R6Tx0P0QPVl%2F9fwiCRpMMGYYZHDftcGmjzi7KIDvTYEG1YODHW2kk3ksQErFEpZ9Elew11rccSXQfKUvfctARCLaxb549NdvqNIRc0spUP8qUdVqnR9xQHCu7fFYXgR6vVvM09MIeiockGOqUBkZBUu4NGnL06altAfUWTTSbjflQu75GAv28Jhp%2Fxh6bIf%2BsFY%2F5tWYD5i%2BdxY7UYGGJw0DleQ%2Badaow%2BfB2YiXaS49gND%2FzrBJvd4ZtccD6%2FsDFkZH%2FueqsiMN%2F1naSH5HB7v3pF8bU4Dm%2ByTOIxXX8%2B9nih0dY5Qr3F%2FOrkhHZz9us20fD%2FfyxMJgh%2FXbWZ47G1TLZ6mhZnFIZt5uw%2BDhsSgZct&X-Amz-Signature=1860ea61346cacc4cc4801b09e9807b56c0cbbd2a2b0759107a68dd2926292bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

