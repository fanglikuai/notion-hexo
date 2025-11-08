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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GEDTNOM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJHMEUCIF8oXOhN1xTeLCv4JD%2BkRIfZzt8zlbJc8oWfFWW1%2BFY2AiEAvDakQ4dSz5viMTHe8rPMoSr%2B%2FoTVuAKh%2FBqChWfBpH8qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2Bp%2F6TQsyJSGpA0BCrcA3xq%2B7e41aYxILYPCadbLblAc3tO5oZY5%2FrWkfKeOwHPd2qEMjfQ9mPf8qYvtK5iobmvN2mF3DdxHfSBBafQxKowg7vO82Cr57IOlLxN5nAMcz8kv8nuMUbRWE40mWaSBO6LgSKcj4JJN9RU1nbCohpid%2B%2BVK7UvGMyxOLxeK4M%2BiFQgFUPe8fBNudXwJtuXvmdPDhqzcjQ96UQbT7bTnKIv1ukGfkfpkNSfyCVNIx0uxrUX6zTDxvb70G%2BREVom3hzRulIy9NwEJRDTZBXL%2FUSGPSrgraTzOIngURwYZVxOU22K52HcZZZfJ15gsnYsOSNlP1YVgHeLxMQs8QyZLcDY%2Bgjar9aDIriVld576O5zWusidBywQKyQ%2BGtcMwUx%2BqMltnIJoIke5WUGUPOvTtlpKY1OfrF5n0aETAdNfh6cm%2Fvr8n7ti4JVj2JJ7VpfCrO4LoQHL8f6ONTb6h7I27O2b8IZvxCyIN3EOE%2Ft%2F7QANsvYWt1mPCLu5ktymQsqJaayQpxmyGpXGcqhYSfOJtAFyBo7bToi1RM%2FhmHCYyYWIqZWxVCpkSc7O8%2BajKMxFkqSPQxIylWpmKxoNuiZ2LVGCxZPmH2ltNkHRo%2B2CsKPUadrTDKToOb%2BYuSnMKKTu8gGOqUBDE3%2BMopCmghHtn88XSMAHnhPlR%2FD9MQFHiZrdhxGjR9B7zpn2BN9qloKIfRdEbHym1UvA5s%2BILib11przRUngyfsO1h8nk%2BgeJMg81qAY6nNYyGGLUAbpAVlo4ZOvyCTNPXwOkzQG5yov1ods%2BpPNy9MF8y1oE0y8ffscjMllZiCV0uPQrD%2F9T0xfQuT96Q7YYTf3gpfekhEY8wfzBbWY3gNiMPN&X-Amz-Signature=c377cff2570999cb43b5235d716c126fce9762efd015dfe794bd791b7ae0b53c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

