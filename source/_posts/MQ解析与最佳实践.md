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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46665J7UW7D%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIBQ9j5AlF0Dh2gRVaH5CpPTJZs%2FNZT%2FoLWt7sIPTmJWiAiEA2dtZSKr%2FHAsBC1wthKyTRbuCXCIw09vTAzjyIFx2MGcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKy3%2Bi4OrL%2FTxcPdNyrcA5%2F5vo%2BHjW3fJz8h%2BzXDL2fNfGK3pcCUTRAkYcgf1Mk73Z%2FS96s7mxoj26bPQ501V94YN5Z7fY0g%2Fc%2B8njLy35Ckz9SigpYRg%2FTMuF%2F3b8ZPc6XRgDr77J9uYnpVEUZ%2FhEYsyj4gKGwt1iE7lm4umkDpywrMSjxvU9USph4EqolWeresDCSokRmJlwmV7CYzO50PIQrs%2Fd78%2B0%2FlK7hWmg2uuaFvAKKu4RxLtuZVfzA9HiWVPm0Wqz894nZmoRjCdzzWP4bmhMHCzPTZcYmsctIVJrEAqcFTAOd8dYHOLXjOaM684CKBR0Z4uNrybsj%2Bm1X7%2F4QMP2kmpIHviZ%2BVjOl5AGsICQcAEIxBkrwM5rV5iWg1mWCWa12MpLmxNGsTgC3jDVMvMCMDNusRlk0k0KelnvdB9BD9g3%2FIwML%2F0KntIVEj%2FElJyl0TSt0d7V9D6k4OQbeifZeapi18V3JKVqZMvnadRKfX21Zt124dgwhOh2re0smcIAyzLcqGtt2KZa7%2FjVvEB2nTgPuV%2FA%2FwIDYINNmRlDUiV9DR6UJGjaS7jdJFgOPp2ZludjOtD41xFSFwbq0P%2F08bAYgf9TAqlxdUsf7aNKdLo%2BEi0WblDASS2aPs4TXZYtIrbtI7MJmHt8YGOqUBTOt2lEx1HAlJ%2F7zKYfUKWFqC6wgbELY9KTpyAMi74ehWZbu2sZkzFjS2gRuCk5lvxh53xc0FdqSiwqDEsSXz9rM7BgMqKNbaRiezkH3s%2F6yj4DuhgXNxadUmfrH90z1t5L7dGyI%2B850ErSuizp%2FYORY0lEA6RXTopYY5oCUXvBPEeNQOuq9A%2FLaS2qo3FFJHsPHFiPrH3qe2n8Z0bNK3o2H4XDGj&X-Amz-Signature=316f87db578026571250842bd3240d1e80045fe1ed55c69757c0d2d6ceaab553&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

