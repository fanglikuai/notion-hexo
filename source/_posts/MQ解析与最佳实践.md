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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLXQ773P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T200050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAECG4%2BhQ4gbAvxh703qZi%2BXPfNM0x2EkJc3oX6meLbUAiBcFCK5Iw62JGoqIaEPmAbA7nWGJJ2dTHTZgVu4tjfpJCr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMgowRp4FKPVxbZchKKtwDDek7HS00p%2FdKiH0HByISbnBTl3fWtJcLYfFKE48YjkNYjLcuMeVUC%2BpjgmYMcWHau7pNqeqQ6wenXJ8rctmBralXRzUSYdviMfsZdttgW0so6a%2BG9H9uVhFTWBIWSqlqwTzvY5OFH0Ygd3avWlAbZlUHyWcIqwv3AlCh7%2FkVtJ2%2FaFjO19SGky4RmJYg%2FRBu2HZssm9jr7LQFhuFZtQTZ98jgK0j4iPqG2iOa5GP4UxXoKdZxsakU7hZo777PPGC6I7qNOAeCEL%2F23j8Ws70ifzzGc%2BTIhkr%2FLk2EVg%2Bvft5734FVrwRRvrA%2FHi7sAolXG22eFM7hvqU8iIC3piw0ZVTurOWXrL3BpldY%2B%2F%2F49Xc6XGgCNU8MpBScOeIE2nXhigGv2r38QUxbL82ZC2X894fxZoOzq9BZMZffr3jE8D0wx0i%2Buy5c%2Fg12kq43qf2NX2s3x43e12iB6cuZnGsYn%2BOsAxN0BcyvHd5EVHk3r4fogxyoUeblXgaBCGzCyeHxyNuvjeqajNsH2qMqGPv5eBqZPGOWkxbEgSnZj73UYkRp%2FGjHjfbia6wfhWoCdrJRilrvYI2mP085cq4qHWelZcGKKGlHupGfdVqNVcvzCzuUBbLWTQMc%2B39TrEwupDvxwY6pgEDLk%2BSMYNyeO9S4PbYrzycH09R0Pav87jVmxYFRjSP6MPKuXQhl2BTNy%2F93g7d3WH8LRlzOzUM7knt2L8zPAkdAZTID1fhfptemKoS6nYfiMzfqSbHivXs10uzMD2cNIvHzvkCioQI8ZYMddT86DG8jeyXnWZk0VAJU26cMEXWZPbxwTP2owKYXRn7vOcTdYaQJ8aJz9dLOXXsD1n8KKq%2F5%2BjFJnfy&X-Amz-Signature=18f4f7b859368712e42769958ce0615236e62cc6d012fa3dbca89bae0ff2fd10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

