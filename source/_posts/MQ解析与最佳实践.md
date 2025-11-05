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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCIPOSRH%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCyEv6aAVkkZ1WH3WgJS%2BJuSj9JMATFoOsOSrevmdjjagIgekUQhpvuc3ty7oAhTaIYkXIadvvhiS6QiTMCGIHS%2FTIqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKgrkua09hHgwnWlgSrcA586USsXUfEvmvRKfUoma1E6pT0GT3I1eaB%2Fetbp7MHkmpkGCEKGyQq6FJRRlzE0Cq0Z0XRBaqsfZtN5gyjxVlxjWRslB5mNASAURQhQKeGbBhidbMz1hpE34H9zckNZ5CbgnCfuXwVpnl0SIwIuv4E%2FUs2kmYDQW%2BOykztTaUBxbdJ%2BoNXu2ZtngqjXNr3zPWKsEqo98hK8Ky7k7Ih%2FvbhWLj44%2BcpROCUd%2Fs08vwRBFnioHT5tTKYHWz%2BIhxoBDVxpHjRWlh3e%2BJ1vAc0tWUvZGvrD5Ie%2F96CDL6p%2B%2FthTNFDAV5FHbfsiJT%2B2DXPcP366V20DfzTWZHra3t2xQjmS773iqrFgUeQNi1pdQiAcQ2P1Clc%2BthMk%2BQ2V2Jvn3TBVTHla2D7ws3%2FR0Jjl5UVIBWrM92QXsodJbBHKmrCCrxLJULwWDdV5jQL%2BAhWhULIswJLrLZClkCO27U4Z5zL5cfgZmCtwkEdT6fClQlz1mZb1pmpeJw0IK0dt0TAmJ4EFE4fBPgOE3xOUlRFN3kjBc2r3OT6601Qf%2FPpID5W5K0ctxiYZ45AAqgQtnJwFx0s%2Fg7G18Hv8V%2FtsntBkcQUS6p4VbyxJjAsZMJckWkcIZ0IVboFV6FXnPmdDMOmTrcgGOqUB0tDWKdBANPNYUrMuzM%2BZ6JXmkGE965%2FpAKWh%2Fn%2Btir%2B1C%2F%2Bl9H%2FHMtcnfW5SNgl6SKsUdtokXng3XyEWqGBCV1%2F%2Flz2DjfzsNBTIQuaaq%2FAG4%2B%2BEmtzl8rZJZBF5PdPYGr8ZdekXd9g2UdkkO9DK3dAY74zwKXdRDFOQR69G0Mnp%2F2KHx%2FZyJ3cwxALM81mOw02GDBaNUFmssXzsMfRYyTGi%2BBee&X-Amz-Signature=cffacd9377dc6c024442834d3317f26c34a06374cc43a4feb6e90eadf552b3eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

