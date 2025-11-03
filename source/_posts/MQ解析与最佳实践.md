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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GSD7VPU%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCOw%2FYK4akTUfv%2BwsBX4VIASYXY%2Bc3yJgKts6xp7kLFcAIhAO5ypZLWvuxqXtND0bmDoAKRJG9enLSnlz9zE3MZnTaoKv8DCFIQABoMNjM3NDIzMTgzODA1Igy11nQH0UUv%2Bs%2FJMIAq3APs3BU4x%2FoJSbCJK2xA5Sa%2Fd2xWZCkOFPgXsb36XjfBQ7bIrfzsacZpaW4B7xOuOIncEQ7mNDFEI4l96ZVjW%2FG%2F5zNp3YZSRi51VxxuiJ2slt%2FG97kJAo%2FapboLKayEiYzjufK8Q10DYzrny2LSAZ%2Bz7IRC7DgDtun3DsalHadQTcEg2gqVnnMzNZ7drkTKI5THpMuAz058Nr8bAZLUD1mvWZ7ElkD7AQitP%2FB62kl3qw0uwZevTHvelp%2B%2FUIAhqyRQyVdpg%2FS0QAthxn8Qcew4pIbECZioBJLISBoPx9HxsgfW5vfkwjdxrLGULcMCVEEpB3rcvGczm5scjlqM1CQQnJ%2BC7gDQW7bFkJ%2FzLyHNjlmbf9RC1t1TugYaCoDPiF1vrWMEez5gz64ZfBUBSaf%2Br9uuzft0nfOBhIbk33Rp40cHsiHphMldSKBAtzvWEwiN2GEOrwbPrZ5N6xm3c7ghJ8%2FbHWfbmyIS2nemY8aN6W9uGdDjR0WXclcGcUUO42u1sQkCf%2BsJt2qM2khSHqoxM8%2Br%2Fu6H0nOv%2F%2F6PryyQxGLyX1%2BDNwnKj0kih9%2FHvnq33mnT68aU3Hj%2FYjiYENb3NOVQx%2FbelD2kw3SokB%2BJU3an2p1i2n0vmpZ91TDL85%2FIBjqkAV5jQvXQ22vSoEsNYrecZf%2FBdB8CTsW0OhC934CZtUe9hrpehZiWiaE4ECOYHIZzn%2B0qb2xV8XD7w2S8n4aVsCx4nSLQA1F2wK89LiS9a%2BLdegvGRSDvV%2BBqD%2FSmy6lWVFrWnqgdOQN54mNuo0V9dOzP3pl0SwRIvk79ctYV4%2FcDFthE07DB6ag3vQ%2B65IDKVQGm4pekiWMKIeJNKPic44wBbN8z&X-Amz-Signature=70b6512ba53815bd41e9a853b7e706962a69e2cc499502c0f8153706a0a3f94b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

