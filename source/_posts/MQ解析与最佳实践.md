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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635FA3UU2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDQbhmqcso3nYsL1riEDTSnKlDfjDWsIiLkoQ%2FFHnQUNAIgccA0ieHjPZDTC79f0mbIDj7O%2FPqINkvDHfi%2FJe6w4SAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA2jWe8LD61fPLXvrSrcA5qEA2WJDGtL0AxtEDEqOMEgeXHXXsxl%2Bnm8%2F2IjBdbEQtC1GXByY5%2BwavLpup6yScZdgGoMlAlhzMjmH6wrTH48eLaVZXvEYJWQHo%2BiGHi6qATCBKldYmbpoD9bCrS6nE4xUwZtawA7cXkc1nIlTbwavQNSP%2BJJcIkN1qXjg35ipXB14CIp0T76XHJPa%2BxUODo6Nlc8li4MMIK9OfGJV%2F2yggGQ7Od6hiyzZOu55zdXv1Blgyn6fnB4znHf9UqA36QXBGhhxugCD%2FQc1D2dZNQMkrpfZy6mJhji%2FknnO9gDzvCwZy7vRWqpsQPnloFYr%2F0j0CoT%2FHm03nOsrybLN1iA%2Bj%2BkswgFpvHed6s6wqDFaKHZ9%2B2K8bDGgPWZb6kY%2BK%2FUIOeQ%2FwdUtGqhdxpBErmzbaSOSsh8YQHaklkZN%2B5wvLM3hJCmivXlORlvWqIKmgjHn3mbAUDJ%2FrPOZ9a4zqz0K2a95TIDSftqqz7JgZSzE4HMJXut%2B0VrZbC2Y3wOTcOApiBDTLVpY7mYnAxw54f%2Bk1rhNnmMkraocGX9Aw9O6HyTyU%2BsNzDZ9SXLrTETd%2FmmOdkZJWs7rw0iKGvyFxIMN1O7XM7t%2FEbQNJ08UgOdngDfqJePwmkxtOS5MISs3McGOqUB%2B8yVZRA5cyeLZw0FuIifcuN9aufYOYw%2BKbDr2OT9tGU4rKFGBWV%2BLPrjH7FN8Sy2jRfuRS9xUHZsrbNcsTFfJpoIIdbKWCENECLppKVOzNbgXCgnJsF1Ic5W0acTZoZaIlJezW2rT6Whgdng4%2Bojb4%2FTAkE954m8wXDKcYyO%2BIlBznAZHi5BsSbViimUL3kMvEbSAtxb9oTQp02QMI0PPQcT1RPs&X-Amz-Signature=2afe04bdb5a20ae0094faea79fad3980304768af531f7d8b63deab250995e3ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

