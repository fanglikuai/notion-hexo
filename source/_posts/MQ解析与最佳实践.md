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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCBPONSP%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDM0rSw5KeFK3JmRp%2FP%2F1%2BXgDOZpv9vVRIbjGav7hG%2F0AiEA%2FTglmc89XERhiX6RiLc7FcHNp0t%2BR3oLdgd9Ga5go9gqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjavvI0yUaZ1rkv4yrcA60U0R3nbvd%2FMB0KSNK9AtDGkz8oFkS%2B7rbeLDvE1M6EHsQq3WCFSMzT5Hvw107Mosb0kOyzkkcNo6eIGj0vSmx2sNzxEaOfw37TXi4TAreYUrTdFkAfQL9j%2B1VOLe8PBiRZc0VwZMLthNJ93OSV4wK92SuUNJTA6MCM3RWxt%2B%2F2hb2pwa8nUbOp8MjQuYjTJn18nJk26vv9zEpjtnRQoHHjkYZCB%2BY30OkpSCBk7IGAQuYxI146cHrQHR5p%2B5qZ88SljjWe94pcgNxaRC5tZHiJ8LcNzpk3XtNSF36XITIxRK%2FS5HSouuC5UaXc5whfE2pL1xt1jeiezCZwwzca0R6zZ2zeH9m6vPkL1GDr7HNfm6uwFNiGuLJwHF0cI9y%2BmnTRnSx7%2FpGU%2FvjHWtStZk707YyDHsGCJUGv4zr90TtE3nE%2F%2F%2BK5XQK73SoNgGT6Rm%2FsT7O6T2iddHKzM4LaIx1eqXz4zz71UJ%2BN2%2Fh3VFK%2BNtNbwwvOEA5qKd3axzFn%2FWdnij1EZszJWsSkF9ecbha1RQ8NfJu%2BvmNLVMaHuxmcbISdlnuKOlHogKA%2FECCt23OAjspBLeMl%2FDUN9%2BiaEOUmCxgUD9DOrTDBz86oqETjIyDSNZLABteM4S8ZMOnU6sYGOqUBLdK8ozFRPSgdQRPryS4N9kvDjhxGdniB1ERGRBboaEDJIxBlvPficiR0tvB2gjJFuPm8AShGq%2FXhdBKMvbRD5%2BQOT7MJO5k4rYe9Vd6ryOHasfjuqAwJ4%2Bm%2BOW5iURIlpK3KYJmdxWaSZUhKXuHMR9lK6Nk4vEphGw%2BTAPDFiZo9ZEqrgauB%2F3Q%2FFJ48l3uNn1sI1HL8378o3ywzjEaDG7%2FamKQ3&X-Amz-Signature=5b51a0bab942e600666975b0cafc2bcbc2357a832dcba8c6b8c9fdb80bff2ea5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

