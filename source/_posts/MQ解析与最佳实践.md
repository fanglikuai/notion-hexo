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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBW5JEZQ%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSPl%2F9wljBKr8erFNFdcRxe62HDWozqntuqcJbicilCwIhAMqj7tkfYw52kF7lWHG2Ajzbois0Ipz%2F1iFRtYXuLQcpKv8DCFEQABoMNjM3NDIzMTgzODA1IgwuBqtu0PQGOxNNJLMq3AOWLzZsByrJ%2BnHlix8wgaGfZV4tAEQxdHiiqNuGnbXz1PDkdn7wpBEMT7ZWj2uKdokMWCpXuvHXbRczd9G7G%2B9Vjrp9HoDsItJDmWl0G6zBmMuiWSw%2FhDBT8zhZFLIR1QBivGy0IGwJGy9%2FhQYdaj7qd8PmdXmbi4ncvk5PSXnWqxaA13S%2FbnRcgy%2BjCIs9TPQiPU%2F74r4ZFTegJ0IxheJLOlrzMGV1lFpXv2ZtvnM4J4t%2BPD3kjFELBe8uXBkBo5q8Lp3fo1Ead2Yvn87D7t%2BCCTO0ywMDK4UQer1JnUm%2Fo%2BLGcQd607mRE7gcEFvsYm%2Bnu1TekIfulUxjTtpO9C1v6KOIbZtNicIdFP%2BmzhGcQeWUas9xtI5y5WkaGQyD5yRNqzVmPqBz8X4B%2FH%2Fdr0mwPZuSufbv60SDWh5j8aWbTcxYVaTw3pcoX4TvnXjarFmUu%2BpAeeewjIr1p%2B6lNbPs1oUxK1TSYr6t%2F9oww5pAVGWDhkz9wAMykinNIRGEVzIhMG3QERzWAA%2BLiDmFrBn5SBqPRqLZEFOVBgcN8D2TgX57X%2BoADIsAj9iPs3kxbmb%2B8Sv0KiQzIL6vWoy1RkzbkBE40T4uhBbvG6Anys4Y1mB%2FBBSPOZ7xY97aMTCSiuvHBjqkARSLe3LZcldd1%2B4fAtp07g%2BSuCmFQvJpBcQE%2BXOIXZqzjVc9xUMe2kmwoaD1KnCOoySyRGHPfgjftJyNEBfvJXbs1RDxmxpDshFAdvOh49QTZoOAcMDcPsj27ZZQurhTK%2FqC7v4QT%2FsLJ18R0ScTEElP%2BYze019lWnikYHdE%2Bv%2F%2B4%2F1VXeTY84PAwH0xpH8a0%2Bt7H5OKT40gUw%2Fr7Sggu6EqGbJ8&X-Amz-Signature=0411b8223e77534554acaf059be57c18764cd9946b649fd696e6c4e10ee1acd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

