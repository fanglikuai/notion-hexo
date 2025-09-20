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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RI6LYSRH%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJHMEUCIQC6K1Ro1vDSuygUoO7X5MyFnkBlGjC%2BiejLqDZkvgziGwIgZ1nt3CBc7hxq7XObQuRpUstLs%2FdR6joNdwlPDYpcmj4qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNkfgqIfbw4Y5SnghSrcAytzTlqVeC2aftb8Dlq4bchtrhgQMEPr7km20LMWinkhgu1SRE7XgUqNCEHSjZw9D5%2BRNSzieLYaSnqnrvMebTvvk3438UaT4Xlfo6QLGJcurXlMjXTAugCpimpfGpHWPA3%2BK716fDn0VkpwkG4sHQLiBUkvlWyJCKEnl%2BfaIO480z7JOLCHa%2B2oAxSin8Y4scnT6Ir%2BaNEoBbBRcbkoHjBrEOLo0SBs8L4FxU898vhoeFfUAwDJL012XzSOZh7YS6EvNgxiOsSuIzeEbtauQMlPQIlxuTxdDyB7vsWnJadKwyJuIwfzg3FXLbuvyUGA1wA5oSbLHaAxZakgxTPZ5OUx%2B93NgrpxOmuYs9Asn%2BbeseXfullSdjxhw%2FHx0mcVsexP1ph9NXqBZA8kncQC2Leybj9%2F1S57bboUurSVD9%2FJDz8NhiKHk5MlvYygby0gimur89z3OQZteW%2B9ishAqc2ju9o%2FPDOeW9QNSwMucf0kfD1TfSdUFcIcdC7sIcnfa9hfOmXOAGvFek%2FvmMw1RLvyUDlYhWDg9Ql67iP0f21TrdzVc0QWgDVGvXKIT4hN4hHGwOhGd2vWCD9H8PWgny5X6U%2Bf6h9xwQDfhocdEOZDpqk76D2ZHYNaROWuMK%2BkucYGOqUB%2FlafnS1aCRWsWrx18KShbgICG0ikB%2B7SkJilN5r6Otlz%2FJ%2BtsOFIk%2Fn76H0mtCpxGSar%2Fv10NdE8ptyF%2BHsoIXCEsr24nYxcCH2OPNUQq%2Bf351tJMSZB54jtcknOhMQHwXJE99EL38v2AGq3PG9YNyUvj0gt8HDansK2%2BzuL0iJSmbAOLLwJL2irYdsxw4QTiyciwgE7jD%2FS4d5hUOoogYG8S8cG&X-Amz-Signature=86433066e843fc923504cae9a6d12d9512f3f3e6cb0c7c1fac78aae9396020ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

