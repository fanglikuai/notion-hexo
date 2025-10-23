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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOKUCCG7%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBeYYIFC57l5lDpXO69qvKLNoYJ8sHzJ%2BqteR6%2FlE5DBAiEAy%2F4i8lNsHNqnK3VYsG9KSuodSVB9Ww1BI3GHrehC9BMq%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDDLeTRFy2SZWhDR%2BmircA4io6zABaaOxlJKr3iVa186SntoEA1RQUMVviNHCIZmVoVtwu2%2B0SRS4cRM%2FsgHJxJOKdbwP6auTYGXAm3rELODzFc1IQE3Qv4%2Bfin%2B2fv4xo08P5ABzXfHJBArgG9uf2vEvsv4pX4m473C3eca9w9pTh67sYSNhKQtvN%2FTp4gutGMS398lySY3%2F0k3MVcdLlaCWTF%2BNohBCltT%2FnXjrjLigL9oCPu6klBA5Pg2O7V9WbA1S5EAHBSeTyHQdNDwVgmewTQQEY8ewez%2Bo%2BHYN0khit751Y8j1yVuNVgPpFsdluGmVOeU4%2BOB7fLO15DpgCARTexff9AkSgIkML55kpaGTlZw4T5HmnIIKsAsSYOpY4uRl8NY9Ugh7%2B9sakaudbZpEVLsi7oJIDgdbS7MmeJ6aRQJ9Ubg0Lx1P7KDkg0LAr1%2FxsC4BRgqaFi%2FnDC8FbOheXm7PMhsQoAP4KIffIY4omFPK6TXXlR3Fcq62uRNtw3qw1cCrZX3sfKak9IB2V%2BN3jdBmwn5bkjuwCx3AQUSn0n9xaxG45ArsgmKRiRKBZNBuSvjWl6wCsoeBj7JIS22u7NlUHcJkNM2g9yVE9kz90LDM4FvCWQ3kjULr1R6m1Mrj%2BKeG%2BeSbigALMJLj6ccGOqUBWD4WIUGVr7%2BXvFCDgwYwtx2MtDlGdJNbpfPjZT7L%2B8p%2FRfKSghnMVW6nRpJaAzu3vEmdip6GaGWf%2BWi4nqLctsqduUNZcuOIN3tkCFcvr4tgsR3hoamcZaf6q0xdSnA6KCaxLnET1zmv6ouC4rdvrDrWjrZGGFCvicA4NO1JfCOPNsQ0BZldzvjy2WucoFIA80%2BKUM8i%2FDP4ar%2FomPegqSn86d2P&X-Amz-Signature=ecdad7635f5164251d619f8bc78ef9ea944d73ec7e84944b39cd47514bd768f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

