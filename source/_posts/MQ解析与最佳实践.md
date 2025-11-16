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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI6LMTX2%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGcNfWSZHH5ihrC6PHAxUX2pgu75vlI0CwKe%2BbmImcIwIgMQaJlXt5EGf4Pdqheiv53Od02zJ%2BHjk1M4tbD9VD4ecqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK797oZhWBWCDoYXECrcAzxRXQDuo2Er6QNdQxMe8cA2tqIafZ4pqI6ox958dLYYmVKOJkYy3cGmfh0uXSBwqbErOqp%2BwKilwHg2tDvhjwsO7nmqx0HlQIV5yvp5PIJF93WEk8DGjpmODu9OZwZpaJEcBZySmEEZfC7Jgl6cVvLNEDD1ShLmvkSGPgR1vGggiExFJJ95RxLiwSN4jWED36JoY1xKWZt3coziwT9Wrl1EGAu9K2aTcH9Fag%2B1S9J916LVOSU2b28brYKyh2J7A%2B61VH5wSV4VVleDkN3A99rAbEeF4LuWEbX2IUM2CO47dlbw90W0LEB4O8Uew1yxIT16Y0Sf9FBhy4B%2FJFrs0%2BR46Wl7Y78xcEOi3fURO%2FKNrnbDACLbrSXHiHc646zFtyCj%2B1fvYUGoLyqENG%2FnXm%2F3r8eCuDpRLZRyAc01%2B5OkGcwqYR50%2BbYxDvlNNqm2zYXlikqGEoDFqn7dhn00tYUfLWI7MaFcBywjcDVfeXS6iMpgxWa8yngoj6AZ6KtW6NBQaQTzV9l%2BNHuWKiIOSse8%2F9QbPTMXcw9B6OY4%2BzOrK3wG3W4ck2UHOl5tdP24zADvxTBKckQgVq3Vi40%2FNZ2xW0Vf4%2FsTBTrFb2CC7dJjzY8dsrIRpnJdpBVzMIrP5MgGOqUBEsGGIfesnEHo7kZVEICL%2BmSNNF%2BKGe8x9NxlQ7DUCWT6RygNpESO4OXDyAWjXrc%2FFMaVSZkzPOCor5bfFXcAW2EZsxQBucrGW76aXcPfbl8ziGdnyo%2Fr4Nm9JNm1%2FagdeLmbub83847hJ15ospCCB4VpHUrlubIhBMBG6ijg7EdO6r5jyO6gWneMVOilu3IkxbLLTpPNNqba78qUgyjEYgr3TKme&X-Amz-Signature=e3dcc6ac9ec7bbd0c0cee07bdfc869b6d402e0a7282070983a12474e8cf5d866&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

