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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FUTKEKB%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCiDmAmcmZEpd8lClem5QUO9gGQRwqMZiaWl0vT8DzPtwIgCHQfM1msf2dI3vngPHm8pAVH1JaMfI9SJxMQJyD1FD4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBttHyQbQ4VnsETWjyrcA6Jy33L6y7YNhz%2FquOvuxMSakykwX%2FbGB1pwwq6w8sWgh2uvJoJ2jMcFFx00BUjFyr8dh%2BsLBj8695ZE2EXz1l8mQftO6sbnmdyrfeN0jFcyHOqzWwb9BLxXxVvuX1Y6DbFwyruHuSh465%2BStHd452ooDavCPrmub6SrWhRZQ8Vb6%2Fz66vJMIWyMkiKntp1wSCg4MY4XvWxB0Fb01CJBbQEJNHxUkLHWDyMEnFspPTpbx8keY10CeP6tp8A09dZHYH4TdUoJCRK5tTkgE9jTVVnh%2Bughb4MXupQNbIoKkuSVrbXjA45kTgpX2bbeglZ1WLYxD6WnZ7KikUHujmjXDDN3rtE0aNyet%2FaDCST9xsRUGwN0yHyxphJz2SGCIaRz1jXM7UTjIZ%2BQEkY1A%2F6dzPfNC10SsJgWJfKGRBgzFxP%2Fq%2Fr3dXXLt9pGaXCg6izso0XZqsSRt8Z4xXHZdtpx%2BWw7%2Ff6YttYyocG4eGS8iygHDDrenxfx7VYub83bt4T5PHLmDg2s6TqqwzUWMPM4Bg7E82uUFbwQtOCDcAbh%2FJCNnx%2BduYxFYBvCFbjqPwFoLLO2r260Ijl7UWzVNDpvhdqxxdC5O6vgbZTRSnd8wNEYkx53HLcOTz04COQDMKKDo8kGOqUBTofNeuCZW1cdWvgZS0UL7VAnPszp6dg%2BBuMa1E1IP3lDQ3%2F8WL7jLyjKdkTM1yWWeIe2nld3CtDXJtWitffuy3UW7DAdkrw0yOWKzJ4oSTj4Ot0Rhe121QllnfV1AvQ8sSpTf1TlVlmh5iCvQT%2FdvOF94zr%2By89RnqkTeqqx7yoOQE5y5nT3F5Wj%2F74N0402hPbumO2Z2spIw3RoEPskCa0RNNN1&X-Amz-Signature=02ed670ff26693ec722e8fd35cfa152b77c21dfefd6aa9eb3057d2fb1084c870&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

