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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VLVLPKB%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTGmtGSrPKvJmnEnK4dPHi%2Bo0jRI3APRa9May6kcgNLQIhAKwmt7AJb3MQjN1m1WisteKRhIQ%2FZ83gxmXIg2w0OUMyKv8DCE8QABoMNjM3NDIzMTgzODA1IgyDmY44goKdN0K9guwq3APb5tO4TlSD8uUwJKABhbh0t4LbKrFJkhcJBKkMrRlzBVrC6FVqUlMVdfttOs%2FsH4EsuAkvapev4GBjS0VkG1rbMJ%2FVaPalWRNWjCD%2BANpCJz3jnMqLOcpYhPy5VFizm%2FpYraAeT%2BP1gNXBKsLY9%2B4arli%2Fk1ijPr9thuCyEtISEqe21jAdPPPPjEb6BVp5Ovw9zRTnuIvID1XUcXOwawe%2BfienDTf%2BfXyUIRz08TFRwzeiuQV%2FHfq%2Fx9ywsjLaLzSN2XqBM1ChTEZ1piIdGW3EvvcrOdZKt21aroeEELeNGUIdmDQjcQhp1U%2F3TmzkBNsPDkM9wCmsS4n54Nk8iRFjbPuwhNXHw7We3AtsTwkZ9tVqBUGT0u7zNLCBlMSsnqkR%2F3znKAflmaYIkzor0Kns2puzo%2BqAs8XgTnCgIWk0je%2FrxwYDLJfJMNrSoI7df1rcwm9GAbVB%2BKnDbySUCJKYiAQtlsvqY3B27F4gU5yX1w%2F5sjt6rYsfl%2BPQBBGumuLnxts%2BHihGHQP15zsdInRb92JATztRRO8TAYepg3d0wI%2F1%2FU8qWo3bxPWHxM2NKlFZXu7gaZRXN6vbvHMDc3A8D%2BPqCambLFsgB2%2BZS%2FTwpUzkEZ38LYD4dMlTaDC%2Fm5%2FIBjqkAW9d6c2vygn31fNdnBY6I9gL3iQeSLp3otyfRoGwkipsx%2FdUO5KZqIBulvKJ%2BKGhp1UH5liN1bbzN06Vc1VErB0v3rSzo1KYvzww407Ik5rVbfmtRiKN4eZsB2pqif1lriJYJi%2BC7AuzveHnoIoRUCDnwPhChyDglv6%2FUw1bnReGB2WwuB3WktdmAzWBFFEfdtgCubd12R3AJsUOM5NmjDF43Iz%2F&X-Amz-Signature=8df0c9a3e44f2e4b4ac3ec937044b49c3952e9d2dfd8cdc2d97fcc70697139b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

