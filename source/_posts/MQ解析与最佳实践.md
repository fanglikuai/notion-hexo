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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OWVBUEF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDxrjI4KHFoufAxRaSWE8aPEw4gmU%2BIux2d%2B%2BkOxSKUbgIgGWgzruVvChakIr23cTuAwK8nYnnp%2Fn%2BjGjsTkbK8U%2BwqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxAeZhnN3q5IkZ%2FJSrcAwmkhDkCXJz7IEPTgKxnM6ilFw4BYmGR%2FLlu1NvJFL6UZFBxpDSSZlkLP26qNS%2FQj%2Bq1Y13o%2FkJdAuqAm6OHKTQgJAx0zXbL6dNd6zNJ9pQOehXJ6OyHeipX9KbZZL8%2F9%2Fb6gZchMF4zJUfMfM12dghxxTBU%2BdCgBhkzcxfUnMwNSlPl0yL6HnnbngFlxseLznyY1q9pXdL6ORLKzXMo2wRykG8FxJkrOVvAXHQmI7A6%2FWt7f5K%2BBC20m2iwnIKT20zpKZ6dACxXiFFRDpozjDGmc4wBGwD6vfBrLKVR1B%2BOdZTErIJlfLM2VkL7YeFHzH1oSDu2W61TIAP7WXV37PRLuLe0pd4vu7mtGmSrVpoCHv0Q2HO0AKYlrEwj2NARk9qAUtwEivCh1r%2FiY06Hfko8jKwm8FQVkBBFSgBPpnBYNsPME9JNyy4fa7FF%2B%2BvskMjUKxBKt6LsSV%2B9demeyYGSPx%2FKxmUQhxi2NCXlH6uf%2F9Z%2Bhp2VFhNmMmI9ieffOvpPv7ynx1%2BhoiCklRjdFaYSWSKRKqlJ%2B3tGuZP6z4cxg4uMBPjK9WZNZ2jCC%2FDF7%2BTYkVrb%2BBfkqhXtzruHDBVfbO0i6X2WNORlTqEX05V5uzUBNeVm%2Bl4GyBv7MMf68MYGOqUBdBs1RszB6AqZS6Yle%2FOrBq1mPh4Bf%2Frj6TkZWl%2B09Smu%2BG24mC8N6xnpWT8pAO9DTMaFiSyP6EOnv0kwLaP65D4NlCmCVCyNqF%2FIkEZxXGeWI1iTPXOKIqgSBF5Tr%2FlDmoYUh9KCvD1pBZTwEn5frgN4GtWwaBo89HmON0QMIEICkwVBcTHyiOfn6q4iXjXvFkhpODs2Nd35Vku9frRE8LM6oPfg&X-Amz-Signature=5ff293142083a699632f6ef8d09f20fd5d0222e647f4381ff5aa5c7f0081582a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

