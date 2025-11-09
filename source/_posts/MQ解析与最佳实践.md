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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VWDH2FV%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJIMEYCIQCbBQT1lPyRxen6eRKcvI%2BXSUXKeX7ORXDubz8NJIW%2BFQIhAMBwJPUygfGzSxBVaqnAK4Xu%2FNWeKJ%2BeHh3ro24bBseRKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIE0KyizJcLAOZYKcq3APzd%2BoyDQCeIAcXyCTKE1AcdzYhy2CQN2iFW4qwCPhZpNrmlFp6Ry931T5RGpGyNm5uNL%2BvR9MtjxtzadScBmS1sszJj0J3CJgRu9kvkNsC5l%2FRF%2F5L1nAr733ExaydgVWkZzIrh1I1lKAC4Qu3ThbPB5BZF5O7M3xH3E%2BwVX%2F1yT0b6hvDdRNEgernmz81suVXrLY21AYDQD7dFADgvz92wqskVLjokLIjJyo5u5ELX5guotP2Ih7YBIAR2LuATCOdJdWF022hHqLE%2FWXggHoOMt9iShXPrFQEtA9DaYL3%2FaqUDg0c1R6TTnWz1SQQY9KG5w93KRfS9cdtoTw9wOZryLBKa5LYAFxjrPzI7UZzLLhjkdeJil6WJ9sDPW8vgCyf4Q6uQOroX1EHfS6SzAhZ7OtcHLsMhhItGeS3eTpnZRxMYeuz5tceUQXggCC4SIgNrvDwprrd2I6u26sNOeI9JCkNpt0ZEE6CsMurh2CKbA7aCdggLOy5dv71NJP9lJadx6nllcvHpvzNzDVd0LBU0MoqzVM2gmvLdKnIozwfxx%2BREjkG%2FmDo1iC7xfXSjH3%2FNp11KmA1pfkyvl8FZ8FoKA5tr7Ey1WQ3SEHS3prdjXROmSCE5op%2BkP2q7DDrpMTIBjqkARqJWT2m7nsnDex1PhS9ZC8177sd%2BVb3Bp3owaMLfsQRlXm1kkKwpcgU6DDGFggOmCPn%2F8nxnD2iMUkBNcczcxabCVQJGUhRb70HaK54L8G45DhsnHrmi8zRZuxHWWWypTM20ACz5UMHLK5fzWr9fxmpw11kdqSUcDxS7hNfnDw68ncNIcd%2BnzdCT%2Fnud8kELZQSQBZkhBfaCqIk6qJb4bzcPl3P&X-Amz-Signature=1f0c98f183af44f561f9f208157cd42d1ac79fdd123cbf7bb35ea9d04873ad4d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

