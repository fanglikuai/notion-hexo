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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356C2PNT%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T140038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKIyldNZBg1aiFgQOjacE%2FBNi7k5sZoqp55SNHfVGI%2FgIhAInSvSklLNLketZEoXYVeyFsywUDYDLO0raBjQBV1nPuKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxnmLofKHmFsTh8Hr4q3APGjlDGdjDZPsChpa16A4iRE5Q6x4Lr%2BKzDJS%2BbETFP5yjceSqXfXhticG7VBlgN%2B%2B4h3q%2Fk1ERb6fip7aUei0zX7Fl4B6b%2Bzuhi6SeWBcRQzY65rGlfzL9z8IccYi%2F1NUmdR%2BVPGBo%2FBZJYzItJ1mX923y7kcTq7Kvg%2Bz6l%2BzHzk81IYqpgWBP%2B9Z5%2F0HVZvJayRex4yw35yyH2FgEN4RFoMRmUGLO3L8gYe50L0xaWlVxb1AWURio%2FrsJ00UmfSHEZ23L8auAMyAWpC0ioZEQsWXjvdvSSGY1m0awdc5yGTb1riVjG1t7EPgXcKuNaFgiUIVZEFSRirmtHuVdZM04qtJ0MhZ3Q2ci3yOioB9ROSW9ZButaUl9rxsH8Ai4g02xver9wDrWVAIE9YUHR%2FTzXyz55NzMyoce45BXgJ7zqCXxNgfXJ3xezrjqIJOhHkBPryGpF73cwM%2Be9X5bcz%2Fj7bepq3re0yCkJOEDje9aCHMeRiGYAZUMSCw1uN5kx69iPZqAlObfm8Y7rOwuUC4FJYatMrr93oLGUTWQZ5zQqahjTlGSvc20j0qTdc3%2BpyENAuRePifRs9p%2FOK7xQbSUEhCHw0HlyPKE1gj0W6C5IG2bYZbTuokNzDH8TDC56cPHBjqkAYW5OKWO8ltlFru7UomSuoVuTaiRdLs7clunjHBROIQd5eELOs67HTnqgRPAiJ0IN2UOIWGR8TgW6lkeMggLBSXxRSDPnMRY%2F1L5hesXQElb%2BLf%2FiIqMO2SRQ427P89ZXy%2FU48xdCSZLTUz1f2EMtRviOmggYJ635%2FYzPdLjY5hNUHRDSshbhVi3zMAivA4Xyy45KfMUeAORP40QocW%2FgUw8gXno&X-Amz-Signature=32e5b3b11ca8b5bd0d8288729f1bb7e26a3f47da8d1a9d4684ad4e7e21105075&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

