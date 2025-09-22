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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ID4NYYX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICRGrzdtRQ3cuVeR9kB1e2XCe00rCCEmCP4BQ4vzQr93AiA%2BSVyzSTFfzn%2B0yFTt6WfPF0LS2p82AiE00m%2ByV737YSr%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMsgwg%2BVObsq4Whbz7KtwDZMbzCXgs30N2wbEuw%2B2oXnpmdxwFl%2FSEuO1Iwh12q42TSY7M6vImv36OxW39MRwpPNc4nr21ScAwYSbhzMMy5VxGPQ7SeLZ%2Br4r8H9N4%2BehxH7t6Jnta2pS6N5JNBW8%2BPYNrd91fDjSjvLzKzUwsiHMr4GofxeGhx94VRvWYU3BV4DY%2FE9zlVFsHp1fgLi71Am44BZA6pPcr1x7lVFcULdzp4LsCCMccI6t%2FYvvbj0Emd8XJeacJzqWzOPGIzb%2B%2BusteucvVFqI6MpsEJF%2FEpTw3SZlo%2FVXB5urC73Zot7IEeDYMLFaMLlZl6nt25Gr7p58zxPV06aeRIQEjNlZviurwlsP4r%2FG4iMPItmCFYqL5UAA0jLhm5zx%2B86kVV6Ed46rZ8%2FTiIBjxpirsrGIjQLNGdtyV%2FaaqDVBjGPTraXuwfZduQMBCIJ1LzIIVapJhsWKj3qr4N1%2BvdYvC5zkZEmwcyCN2TBp7NqoHqx5NHB2cHiNj8UHt3B2nWuSsZbcNCe6k7w9EISr%2BpESB%2FrClK%2BwwAEGdVsZtoI16ujmxz%2BDRUL%2FOrmIkN0JvI4E0Os%2BEMFbKlY9mdhjtskBcSxTkeQHp%2Fm5cOXgb13eEZimANyl2RRfQh%2FgngozX9p4wgdTGxgY6pgGo5iprjS7wzrrNABXneumMtH15snED3HbJ5D18PhbzXXS272yKJCDZifx3gRIyzJ2P%2FSqSIbR5%2B1ICUCJClHQ7MsePDlu5WjkVGTSjv3C4FQc6PBWNmGQztT0g5ZKmEFxxDWDFzyg%2FBCKUZcz4jOcdBjaWdEXkOBgYrOPwpc9P2%2F623r86qBXr9oV8AEK3jWZ5YfsEcvRvPoTISeFtQI0WXolqNBQ2&X-Amz-Signature=75010fb70c1f40e27f5cb43457101ad3116fd46ae77c4825b357554597732fdf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

