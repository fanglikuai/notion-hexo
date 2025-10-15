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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMAODMEV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICKTgkSpcPEUnMal%2FWCLctP2hdxyF0Qi6Swq0zML%2FB5BAiEAqz3jrpXQlAVj%2B%2Bu32Fs6WTsvVt8whQ47fDvsR8r96LUq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDOEpc9PEWtrJJdME5SrcA%2FHJ4QxgW8EXbcVVLJF47NvLcRxeYlBhFrPOjh6DjHIozjW4gQRM8vMkXGvg7Fk3ezDl6nxGOVUGoaVpopap0xS8mG6dbaMll%2B4WbVP%2BXylFwq%2FAbwb6mCLcqya6ZFXN3kpV19hNKUrBsDOuIiY8uOl1VoilOTRyb3l9EP29gHUSYye%2BqGaLSRkjENGaPnOMkl5Lr9LkS5keoaGOn1APs5UKm1LuebCOLmRQlz8ME4xMQ7oNN8xKKqwZVnyk5V7vT8rsyb%2BLJTSFjnb3NBt6AuZnuQwfXmmruM5peU3jx0wT54kNd9QP1e7MIExAXsm9efMfk64zAdfGx%2F7yHkK3TFk0ZfLTKYyYw6ts2%2Bdfo0IrCwBgrxk471zkoZv6we5aTow7BTCdtgqoHHBub6Aj2kP9EfK1uxtMjw4nLVHDg%2FbeSylQFAXVD8TAmjhWUBMGYZ9Ljr4100vPS9I8YQ%2FlIkb2WNixL0cPxr7kvjvMWCmrmh%2B%2F9CBD5qBL1%2BcRcviUly%2FTT0WZqYCyTTJ5wix2w%2BAUXMFJWyGvPtdwnsKvPgV8AOdSRYP4XnJlKF55tqtdtV6wHcoEih%2FT9hLei0YwXxUpiYR%2FHLp2XKIDFkRdhHgPwAxu%2B5UxwuHdszDoMKHRvccGOqUBSXUfY%2BVVrtTuB53nlnF2nnLQPmCMBnqRWMZ%2Bih8wHx3ypsQB5BfgUjlTLN88oU7kHkS56GAk%2BeisZ%2F1jMaRax%2BgtT%2BrswU58yV5THyx8De1X34yGBRzjJWBITIxOTHk5jxE2oG5SF9o24wkWzouDiDmwEb0NajGjkTwwGVbo07vs3csoYuArw28UEPxPggfdIRT14rQg3cnzNbymExcArO0A5GJ%2B&X-Amz-Signature=314c21212dc3ae1a850400cc78fa66f48de4c0eee2cc1cfc2888fdaae57184cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

