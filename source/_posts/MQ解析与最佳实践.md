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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662I4NGNP3%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCGYrd1NXUdl%2BID11HVzbYrE4Dr5bCbcCYv%2FT9JyGqavwIhAM7Dcg14zMfQG%2B77EBVdCtFuzBeACsLQ8r6zWqs715kLKv8DCGoQABoMNjM3NDIzMTgzODA1Igyql6yfKXXSFv1dWs4q3APS2sanrRIbVSngodVhDllkUnLDbep1Jg7ccdJ4BKbcdbP3zW1kSj%2FJD%2BogJmaFmjs%2FMyxQ3GXnigsEDBno1I8akkHn4WOxtvJ5IC1olDQwHRkfF%2FFpHH7jrZ8%2Fm4hT5aSgIn7%2BG7LEprfaZyg4%2Be6g60I7FjpCRTKsvoUuWm3wkwiorkNwLcOBcUpqb1p63u%2FzPOKAQKaT1D6EPIWFn6NqiKZlLQ7hAKmMZSKZgUc84lYaJivS0svhRbJfFJnt5OyLb%2FxacJszum5WHJtArTziASacOo4ap4PDhSsmlbH%2FKRBMHE2QLk6pyQZdxoGXd3nzLW37UiT9797QpWppamPFgjgMtxyEgljFxuIAmcIIo5AQyoxtinR0iKRB%2BNZ%2Fd2w1aNhe9JNEOiC152Xnd%2FXsAmG%2BPJagtSRAuDNNWiM6G%2FzrgZlIV0cbAWliLuWUoeJ7hdxff5oOPs6BXHxS5x0j6nQXu5B52%2FixO9IIX3xkuWl0zvhYL8kp9WcFPRRXEJfFmtuSEtUVxAx%2BMtvudzMYiref6huIw65%2FSYLvSq0yRbkiREXRdeRQPuFPlx7LCnmzdQd7GmCmHr%2BFp7etsAPFi2eJhtYqFr3uFfHw%2BJnvoSnCNKuN8UgB1xYGlDCrnaXIBjqkAR6wgBa7hR1ZvwrhZznGaHjPLIB%2BwOgU9%2BMDIUV5eryIiM4niWpN8bvAxRz9ZkNSvVPtb6R4vSn%2BfJ78vh15D4w8g5ToXjN%2FDhJ9fQjssTrExVLiSGcP3gRIkK3dzhdRnhV%2BVZA6sZJrINzoCNvB8kmWnnLnCQV7OU%2BwoxnvrJPZm3NPfnnvZz1o0aVFWqayqRvXg9yeLt88LfuohBOcR9E%2BP1bw&X-Amz-Signature=75c8fe64ef776747cf2b084b72e6335a663b652a851774cdcdcd549fff17a396&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

