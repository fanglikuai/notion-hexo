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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6WIGS5C%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIHq6Th7Rm5WMONTb0aaBvhzOm0S7tlR2B12h%2FNZ5ytCjAiEA2%2BWxHnDaXuvzjPmPRqj6Uk%2F9Hq%2BtC3H4BVUZgONE7FAqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAxpj%2FHkWSdLP%2FUo8CrcA9EJulhswW%2FkPAThNUYNNMsh2j2CL1BhGHEdH3k3zf6aGpd7B0xUT4WcY1sYOmned2yY377V%2B4oljs2DDnbrM5GA9ymJLKyYz%2FTV9WjljAZeCooFLJHS%2BSOgcowAdvrTv9sTlzTH0jZUdHeitoM5VX2wFZnwqGOA31d8nxrqf3GcyAvQPsgV9Wr4oqID2ojksZRImcUK%2BjZoMOZneYgvb0FdXoVWCYpYx4s5mHvc6d%2BRgn3g6pHUj1cIu4TqgIlX6zO%2BJuGlrc1H0Yz6KyV7CdZq3TqWwGkfIaH7D32HVFu3Wq%2FZoQD7XtQ7KWz0BTqQ%2BsZI64Vi2hgEXEC3uGilVo96%2B6bNY2sI4r1WGwAeckN3MRWNfUwJ1uyiabgw8WfHwiG21SUEFXvAEfpji5AJ54jUuUCqLmnuKF%2FiohYTEjO%2BfxQuYkRX1To1fXlHNf6fHpUWhuStmcNzyirFm2C%2BAYmW38xSERh05JccD7q2UZ473ODTWOmiRhCZTc01dMhHkjq%2B0X2putWwPFvBXAd3z4CeFsiaGKoTbOcfwB6tdalHytuLFlcqQwFvOfOcevsY6kXH5wiaNUi6uQQHxrFdtkOj2WdvRm4ycK4VuiY%2B3x2pRqWCJOwyzkrENcf7MKiAm8cGOqUBCQm%2Ff7ap5hPxok5GNkHd5WlS2hX63v80t4KfP7kMqtb6DedpzoLpu3Cwt5y6b2nVtHvCcnOs8kqqCTu7vGOCsgOahKAiI5rPLNKRghOUU2NHcRxjPwGK4maUpz3DsfuKLU8H3cbnM4cPslhIVjPeAI8p%2BZPaeDjZruJOp3N6a4OQAdKeVF0OEdIKGo2BO2fbnxSmKvWBWEM%2B8AfsgKLywU2r3UY6&X-Amz-Signature=af55b8c8593d9a798c1a2290d5374e528382a813d09917f709f5d84eb51ef449&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

