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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AOJI7ZG%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQDa9Nn6eBaAJ2QLCf%2FdN9z9J2m3EK7KLvaTm44tsAySdwIgPrilRDvl3OLngSOfw5%2BCQPSJ4qSLQwDTlagsB2tGtuUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPekwvHSJPisrbILIircA8KEl%2FuWZpFdrz%2BVAmIIJeP6%2FRBnVjyrWMckQVDIn1nBTUQIYCxad5Urf1Lg6rQgnk2mEXPAjePNwIm8qo7PIQAgwls5tAuW%2F6mst%2BFJ9qY0HOpe8rRilVvaTu2nQXL9QoyIt2iK9ETIG3nnZ0vXaUAp2cC9tfd6RqOYJsHO8il5ZmMaBsu%2F8u4yckraw30byXAX4wozEm5JjJZp6zeNlPa2RAOAr9hlRIxZ9g33rBXMm0gyBv9rMNeA9sM9Bm3L%2B6NNYs5tJUAHXqE6RAwTPl5WQRvcfWXp8pziprJ9jWZfG9fxXHdlcMAvVp8jTBUQ9iKu3sfrUIVPq9AyHO6sNYvmwLihY%2F1cKgQ2cuDESzi5%2BVvhbED6o5Fp9whH3rItk9%2Bo9dv665BzO%2FUdKUwMtW4yzlqekgEfNVI2CHH31XAXUdu%2FtfF519i%2BFIqafgT3SS%2F92d9U9llB1adP2KHXOTo4oJb2sScnQ57t22s7s3t81bY7HlIQy59GheTI5LIP%2FQtNZeFkoqBn55kCmhC33%2FeH5S4mRIwxgXI%2BQ6wWatF2VRGA%2FGTOnizUNkeRxp58f3ns8rG28Eldu7RFDsdJJUYGaMxZF%2FS%2FwY%2BL%2BuNjYf5VGrd8juX2cJuPuyNXMM7U6sYGOqUBZjTN5DAo%2F4S49nCD2LKjvcW7l1yBtxD%2FN4mtOtj0s%2BgPG8FdsTyWiYReHw%2Be4CywwwOVrItgBPXMrukZw2u1DOvO4BsbPtjoHPB2rx%2BCcPQwe8q9x3Jvici14aKHg76jn3K5X%2BQFG5hDaUNnLpBOXF1IuwGaaNl82OTV4Nq%2Bxvy8L5Bqxo59hQKrMqJjGDBb%2B4tW%2FEYwqNeipuJJ4TtcshFwBlZA&X-Amz-Signature=b4fb56d45325ba513c6f704133d8d7dfa799b6a74db2524e08d430b315aed3c7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

