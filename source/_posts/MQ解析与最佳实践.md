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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q574Q32N%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQC4N%2FEacmIZEnx9Ss7tHwNFfc1fZaaXP874mYY4ww2CSAIgQxAK9JqdfPWCbp2iOc8o9FOreqiI6t6NkdmfJdcfO60qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLLIFl2R3BFwNgmv5SrcA%2BMtgOp4LxseLrhAJlJVy%2BumI2Ttp2QIm%2FGpUTmyZO2itfP9Tzuej9L9KQ0M%2B1Ga6W1zS4npOFsNH80kMZFEtH5DlytGpX%2BOrAGQ4vifgDQRQu86RryxG0gwflHtIYCtXZVuRPXSEqEUao%2FZRYhUqlCDiyEX9Djx81ygbZY9fsehwALxKajLbfEu2G%2F4awSb8E2%2FrdwttBCi%2FaD1QCO5i2uf%2B7Y1US748bSl6eWEGULMpwP9%2Foo7gNXE8%2BSsGyABW3wdivkMUVZZ9VZcaAxgEVMBqQ3BrVxre6VFTIOpnD0XI8ifHv3tDSPwbQcd8w0c%2FLNUUaBBo0r2uA4gjTUtxuPBkmmo0I7ganXg7lYGmEMQjySZiEhycgawD1C%2BArfKFYefjtxuAfP5hoOrmwoBnfktXjyy%2FZ4aB2dc2wA2sFqAaqXJLV2OmHPyX9tKbX0ZYx4U2OB8S2ZcYA7eFzOfDqObIwAhb2BU1hKw%2BNPyLbp4YI8eGvczK%2FyOREQHXOcWvW7CVhia0VCAucQnhrcntC96t1d3rFu8SEHa5u85KeS13Fcg9hMXKCFOlXSuDbEeiulP6BjvzLGTaileZwhRMH7eNPwyVrHOGNh8RQSfQjML2PD3s194aZxC9MU%2FMJyG7sYGOqUB7OxL3uIWuk%2BZlbIN958gmC55FuHax7kul%2BHtInuIG%2F3lysufENumBFM4qG826qycKedSAd%2Fhzb026UCguvnywkSo8cHgrMhBAJFLENNdCJ3Gu8CIsjWLbUiHmGA7gwdpYVU43d6jYYYsQnaAxNhHNSwOxtty2b1R5aMgOX9oG2Z%2BfSkUoNFowFAypWzPDdORmg%2FEbU%2Fbbpb%2BrLa5iINGemEkAhKh&X-Amz-Signature=4ee710e2aae81ee34e46969d2f015fe70a1e8c4351c780e286e7eee678587f22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

