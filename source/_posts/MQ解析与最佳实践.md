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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QDLIUU2%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJHMEUCIQCjlvxH9dVl%2F1VFFdzguSV1s7vMTJyeghsdnYWC1XaaGQIgELaIdV3lPapDJbEtrOqMkuLnmNGKN5K5dFLwG6Vghw0qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIkGeo4NzfINjkLDfyrcAx6DBk%2B%2FMgSnYja%2Bs181GL1fLre9OnHTRNrmjFAp9eQsk3TYG6Eqq9ASr5Y%2FWsFO5VGkYSZrIbxzqgdDhp%2FM%2BFCeKQZoHX593PhWubQkJay4AU7M1WVL6Oo6OQyoL%2BBwpTMQ%2Fu3yJnxg9Xa7A0295HZCPzGshyuUC1ErSVQjZVlF3tNmlDrQ1ILE6NCE0OryiPxVKJkh1RYx%2BTt59tN3JPdJTDPrUKaPueIfIU1MuxFMLNYVXsxydupSQvA4g%2Bh2%2F%2F7tzPkryKa8b0GU5kVWamXyYovMyHDfITyb%2FWy68G0lS4a4oUWIpl0oUqBaNmJdC2yqBxBAGw8X%2Fi31f7c%2BxZAaj59w4SSpwe7DChbZuGp5aO3vnpFL7jXN3YI4reKvXHp8c6ccr44S8FIwTy7Y8SCy1BkCQWBaLM2XrESvwTH7sOnVqlFgTnRLt4pi7zBZBgbsuyBLvjIT0zeFd6PUhY7iu0FvPbnA7hukfJaUs0a8TiM047181Z482hUfOZyeoxPZd%2BFglGRqwRe07ge33QNdbh0EyOM1vvD08fi8x3aPzqcOKcYniXbNhxnXsTVjbdayxJSBwq35vRlZR5w1y3yIEnDcWv581N5o5ZsdLKV8RUeWGOBrjGZSHY6HMOT788gGOqUBeEsqPpe%2BPPuSBK%2FhEszQjAgsyrtwDRkVGCeizFfvcHcB3W3REO5U1PJmu5aBRFCbYQ985P%2FaxNbtFT6hY2QNc2FPVeAFBI9n2%2BGnIr12JhwPdS8SILqow%2BlzMe8olqRwe1hH5uovph2iU%2Fe4EO3BQgi0FDyI7eTtzT%2FloqURXDLEValZ00u97VAY7crjnqK5MRyFAcTJ%2FvuEt3gcrsgH8by7A0sL&X-Amz-Signature=b17d07c30df700a141f9e8e9c15710fb40d8454652f992190e688de19175e921&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

