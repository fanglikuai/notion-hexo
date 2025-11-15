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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667N3MWM6U%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC8OCvgSU8YErJiLgQZNEe9%2Fd7wRu1r20nvtU9CSYMr%2FAiEA1HQcpoLsqlbMVca92gVEhiL%2BtdakM8TiGNJKsbzNYdYqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsHTw%2BTIwMNBt7kjyrcA121T%2BZE4Yx%2FcwUvA6B6SC15oIm%2FFx4LThMkTraC4NBf5iiAaRk9i59xG7zdQlKVv3YI9CtVZ4Nr6HrY8zszVgshdMCS8Ela1Rxgw7kQcxgaK80ykFugEUZA3LNTxuELxhIlOLIQ1K%2Bdn6UfaG1xKO2YQpUdA0RIpUZl9NUY1yS%2FdATXAto6uLLmcjo7yHJykys2zKKaJvJfcmDReM6fO5RkT%2BbRr6%2F000IaIZ9vJNkX4k0vmTLJOS%2B9fc0dNUNOXcf%2BHfw2OoUg6Xrhttza%2FSBFcK9P8rg0do8KfZSUB3kgA8R58rjWrov%2FTgmeUbNxIBvi1rRhQmNegRAuJwyY1koCqRvISYKtVh0Blk6mMf%2FKflBeDOdXL5BNYZWXAviskHWmqMiqAwvEgS6X%2Bligr5hnx6mQNcTGRCc3NgwjRbmEw%2FwSy9DuELmlRfaTXT1jWdjaZpD7qyWYmwYh4IcH1i7R2NoqhRJjQR1Kc9oGmp%2FrunvC%2BUedOlOPVBL%2BJVO6BF2O3aHY2CDI0qtOAxU2ZrBZ6QoewybQ5GRUmSLPQu5Q2p49RVluwN5LkbDDcVfJmRYYyHLBbRs8XWWLnQ5Lwk71ACw8hZoFjGMSZomTYTqpj83XdgV%2BubhkNWeFMLTE48gGOqUBfUZoHRYOCkSaMUwbczVdgMZlCZNrq%2FoPSRvFRDCG1UCmIGnhf7KThXGxtj1rI9ataA1WMgng1HkMVoEyN%2FszCK6IBnrvomT0igiMTg9pdVH5fjdcdh2Xsb3fslnWllBrIW75gGxTLDVOW3fEKvLDX0RpELuqFpSC%2FmhAnMvvd%2F41arQ4wypzu2UX13R1Zp0no9M1BQkt450fZ62j%2BdJ107OU0CuM&X-Amz-Signature=e235e1f53c39d0be705fa655b38250cfc65c072dadcfb308610882e9bb7fd85e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

