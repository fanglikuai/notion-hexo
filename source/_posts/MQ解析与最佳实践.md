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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUE5342A%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIHEOlYTlqnM9ryNzdGfLjW9IeZDzRtBGZaolAX8p68mjAiEA3La6qxz7qkrdUoGVeVSWArjm16MqJkRXtOTNdpWCEEUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsU0R6P0Qriv1oCZSrcA9MK20qPijQ04u1wlHU4dB6keJk63CLxzJ14trb%2BL07VsF%2BYBbHoDPHN3QN7Gu6SivAr9hW62g8%2BlVMU9bRByM%2FmLTMqkCLQx8Wb9d606cP5dj9NdmgY3vYznE%2BuW8227gAMPjPug41nFxUEJ%2B%2Blk4G65adzOElmbvmq17dwEKZKqO%2BFYDo99Vzp6f5yt%2F2bnbDQm9nlZymT472W5RKu6IrE%2FW8rtRx%2FcRTWEF2ApRgOFsSnrSay7R6%2B8n%2FhgablpTYmm6s4DU8AFsFKC3hWIHoDm2I%2Flj8dHSRRXGGveONRQ%2BbT2GcS%2FOIqGny1mDhZW7KmS%2FxitiFPn%2FqUxhx6AHzXBkCJ55r5KnF2SJuCNM9B0WspOBCGYzu3itru6ScXgSuv3N41XO%2FxVrXrgzh7%2BEx0Sa5%2BvsdRU8fuDvxaT%2Bjar3lIa9njpxJT1%2FZ2KyTVt0gBdB%2B6MIIU4UPAqhi0updbvpZZu9jXQxESt%2FkUyxPrUQvZPnft%2BM%2B0LeixCeCkdwAH853K%2B%2F84K0XhloDRME5JnijIbeqefhqKRzKG89ElPVQo7c5undePpNkUrBnhSbc0VUo6ujPA281C%2BTos%2FJlp2Yl276fOPxlDNjA4M%2FQA8Uav36%2FoXhTiWekVMMCf2cYGOqUBhOpVDmMIAwThsuLjc5z4kG%2F1q4tBXVOT%2FGTD%2FdtZHKxSjOVjAVczkShY9X7JioayCdF80YH5GV6PGRaMy3oJZFLhDXkbTCmwPbzDrLxkJCVclE8PN2A6zmnmIROOkkgRArdUIyrvUbPhMzv3qD5NgunHofLOBv45U99eFSctGNflomfopy0kPBH4XuErH7vvFrvO7ro9vO6gZVJ%2Ff93T2SlgxBkl&X-Amz-Signature=7688ed6785e2b4211055446802eb150e0d3c0ee303ef9269006e30495bc3e052&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

