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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662LZNTNL%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCec%2FzZXc9hbOPws0X%2BZqVQyId%2Bn43DBWUcImk0DnUsvAIgO7w9uIL0XE6XrATQzGwuma2FwoX4kI6I1qEIgq%2FuJScqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHmRWnvUG4l2by7H2CrcA6N2yTjmETbj2xwsnIfeEb3WyXh633fGlLbIwIA93mFnsVv%2BH%2B54R1PgksrHf6a91MnhYegaN6duLIOZ%2FqZCh3yKlwxDj%2B5eVuyxDdeAxcNndFpuQcGC5iSXCipyrCj2wDLeyFWu4AXKb95mhIonVh91QaXo0rGg7LEJC0SxB9o8abUZnP5PKMX0Q%2FNbxaV0rSdZMC%2BMnUJ0t%2BaFoViPXmsTOh0VViYtoYgM3YrlB5z6jhFu6ef%2BB29oiHyJ37PJPdZ0pEPrx8tTxrxZkFM2S%2BUKHEnDbKBDzBJxli8dWiVExEPfI5s7Te8Y8NPf2A3SYgyPCddVXSWVP69H%2FKUUsEjnMWqeFcH8W3Ms9bcbvc%2BxxWQ5LQGNMXTaQFYkMcWbsBysboOhjVb9q04Y%2BOB6ifN8m4amfMWLaR8OWp7pst9JENy%2FOBbB3TNIZ26lsnP8kqPfTM4FK%2FGAUw8uXUb1aZyNbpuGQxl90wtGjGW1s6XsHYpV4PNiWIclwGLZ0oXdO1Pf668S215RsxQOSGpqILrteOegR4eJsS%2FlmQqYCyvodCCoW4hDolIDMvmwZhMYlBj7E1mxeSgMakYidO1frxLrGQAOJdbbYVsv85JIS4V9ffY1xo8f%2FodA%2FgxQMNP6tcgGOqUBqVIzB91E4btINGY5PVCJCJqT4I66G7pFPeLQ0S14C1nwZx6lAmzn1vp%2BKNktAx3oLg%2BpsCMbbDIBSc%2FQndz68OZ8OHZ%2FMp6PYD1gNs07PDRtSiMkVaugdtKlIPQuAwxqq1tOvBwEJ2wAD1326HfURSpIXXXEaKSLL0ocUk3%2FtOCS5BwZ%2BR1Yz%2BS5Ch%2FirREor1FKPLPsEpUKlcQQ98TGGCa9o4PC&X-Amz-Signature=53b1d18e5e939af2c905e516eec4261fd919a52e0296c8edd320e2bdb040f403&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

