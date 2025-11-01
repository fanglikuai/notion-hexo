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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROXWW4ZL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQCg54wyZkn62eOKr8LQr4LtQknFo28tBDLxKibj%2FQoY6wIgfKBWrqJCeCD6k3llKGjmPaG1PN%2Fl%2BpVYTG8T9b9z2Dkq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDBOyxsY9qUpTFFYSnircA%2Fq%2BkfnWhwJf%2BStu27XdbVyNznN2udm7hPFiMY6xUwmD1V5jy5zDExro86OQg9e%2BLRLNNm440OdIAuvbZmuIr5%2FY0bbMv1BUBPQJLjLeyrTcQWNFC5bKHYdPeAAnRM34V3TAJVXIL4k3YGq9%2FVW%2BW4oRnlk5nT%2F9tBxm46SCVST%2B86TDyV0p1rDxkMyRooC0%2F9Hct2AweheKMb7Bk%2FJ91T%2B3M54aP4yM2x%2F6LOsfTs%2FXhexDW0unYPblcNX3TUWcH4pR2fONiP7conMMAwLkp5DyscprIMuS2%2FQrpMeBZMv%2B8gZZv7k4nCafa2z6z8U4d2O4lzJeujCVn7VzktOo2LHrcNa0cxzHplWTZQu2F6Dh9FSXeKZ4TrV1ynUntFEEMBjYttx0rBb33o6zBN2xnVhHOTQw%2Fd9IBnOEqYVcNXyAgfCNwXJlALhJ0SKDnAAG6smQAiPSlYY5BOZuvhfnL5%2BHKbscTL0oLW3A50tuNlEH9PAZbdVOtaRqbJYKOd4q3lunHa7L20SBDc0LoHGVQE%2F8qoOOe899lRTC6xz64Aavn9rNzUYy%2Fr7Tg9JxmLBielizqWYqnGevG7X2DxROEXHMKUowsC8cnHpSn9YY%2FDdBhW4W7zjOEqHH%2F4fgMIDQlsgGOqUB6FM8b8BCPgLe%2BtnLPNTbiyNG3jk1esn9ms58ukP1LIrh%2BzDkdcPbSwaUlUPpvUcldDXlnlbWwE1KZiOMAEidGUxSFqJ89hPkNu3HtXb5oMIURocCGUzuSJEFHCS0BsiH2HQCVMEU8iHXXQprV0vCqfiYRDmcmH%2BpvKUfPgnhwl6obg8WRfIHREmVsjOvXIYrmM2twF4j5d6gF7bZd0MMMb7Lv2mL&X-Amz-Signature=fd783e617b73efdd3daee405ac3f5b6f27ea5d2b32665ed145f0523e72bd66d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

