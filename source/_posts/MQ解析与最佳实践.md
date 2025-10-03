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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JBWM7YR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCPi%2FAlz25wBOmqyBl79RubGb%2FouSZ3xq43eS5bNtvPKgIgA%2BdXgB90dQmNTgBDpODjZ4jV0izaU4VEcoMkbJx1m9Mq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDHPag4LjLxE2XthT9yrcA02Z6SoEplcbFzrdhYRuAKqn6MoqkO1t1QGWNEjj97DUtJhUMhQ4OU%2Bi8whQ8k65PyP%2FvalPLUpYXnbDf5ndNup3FG57i%2FkOwUxHhVow9bQVep7uA6%2FwBdlk047AF4hgQ5GuZ4iZ7JMu74W8Co1wxLBemwYFTqanc1ABtAuLfYgM0E8KTM5Ujc7U30RfQQMYiU7GYwk8zOkA%2BvLDHGTgVH5t7rp60HK1qUTu9ZT%2F0WEtFf3ijR1Ra8VlJws8fevr4GurUNhO%2BKYxp5cZsxrjhF6s%2BB0DMXaI8YYTB34JzIy4J0Wi2ghfw0RGwVBIAYeW0JDML8gFY43%2Fb9x2RCL5%2Bc3iP6bVg0c7AiiWtxC51E9I64c%2FHcyfN%2FAF%2BUvuVjsgZg1kjtU%2F5jp1ChbKvKG151ngmctGxpdKt9fBpOzcFsSkW83Ybppxyqt44vlKhvtXDM0vyn2Aleb5UhqBecnNmjcq1s5FUpy4mdOA%2Fck6YzwOFX4Jww9beRur62%2BmIx92RTq4qa%2Bhxoh%2Fp%2BwyEnnvt1sUJ2geumKBd%2Bibk%2FspOes%2F6A6QImC%2BXbezE0UFFFRKLQu98RZlffWIKUTvtZdMrgrEjm2jGsrcfWAxT0Q2YV8OtZ%2BwibQ0y64gjv9MMMna%2B8YGOqUBwtEs%2BCRWb524SxRWZVIesqqut0evZROAfxSlZH%2Bkfugw9763A1N43AKCgPGgKakZ10L2cKWGocZTPaejbQQ2xVoWGKUd2mc3Ynspd7n1Rk048JDUkWmyyhQBHsLSSx47GrlGwdWRFT2yF8AIVbwPJsfwp%2F4zGxwSwlOSy6lSHQIxt7cc3Knm%2BSjxzjyGE0thXU%2BtOK4%2BeXOewUl7YgN9IUhVr5AN&X-Amz-Signature=4479d82b05a9abc8e3ea73ae5c628744feea22998cd7583f5bf4e1590ba83675&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

