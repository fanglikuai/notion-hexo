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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN5RANMY%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCGEq%2Bjcudng5uETDbAuoFMukIzvIvidz%2BciSrk3HsmjAIgYUqI3eF%2Bij31vJ5jKepRtd%2F6E4J%2BfieIxISFtdGoXcEq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNDD8kBxg%2FCqiGkHHSrcA9JRxbAqXwpYNOBmOZhcIFrNZsg5upPQZ6gKKYwcfRSJl3FvqXXw2aYkSPihjN1jj2UAl2ik1xg8GWbpSlE357LQykiM6qqRIzh3obZder%2BKu1Xm2a8OLvTeKPP9xwhu%2BmCQhzacZRT0g1%2F%2BM76HKu0%2BxIHSy7p8O0LH%2BSnaGXyL8wim06bWkcvtk7CBesXzmlvUZmd%2Bis223FakbjwjSL4uX9oWnFxOfhrKj8iPTzx%2BVP7Rm%2FGqXDfnQMY5d%2Fp5qaaQa4fV3FSyTWgLy4Ob7%2FO9n6%2BYcHmfQuuQGd7sJgyNGMr8ItyR5K2uTI3mBiK5w1puxDp1cdTO3wqNRrWYMsPgbV1ez7%2FznHkJXK%2BcPwLaNA1J%2B139iSSU0fnVRWVS5DYootXfx1d3g87uE435Qpx6EjfUDvqh%2FxjNXUHHguDOFPJBCb0COT%2BRadXPRVl4zhdkpUA5gVByXuoOGHei6N1lQVlMOJ1mEITF4n2mC98VBpUP4YLI1ohviVeUEtYtIYYeKSnQbbhZ%2BU4StAqs4mXR6wRmMnfR6cl8alX3PiZ2Fp0B4I%2FoXbZvgpW9thc15cMHkHqac4dTxEC17MKlweeSd59umi6MV7s65xnDYWeQRKkkq5260a0vblhPMNTTm8gGOqUB8EDHoVqd%2Fmhz%2FY%2FZc7K7Fh2Sk%2FMPBF1b5nWNL2gNbBAiKqG4amXm8qqxjEdzUMl3k11IV8uKrPXp2ydrMP7l%2FtWSRL%2BQZtmkBAsh4W6hHOkJMxsatw3HSNguWZdsREKEfwESlwbMzFkkuVcso5z2BH%2BwlcqEC4S4Dl7LopeW99W7IM6Iv%2FQS4aERPZQjNw4iZmeNiHbpHvfzu5m3MsddMPG%2BrZNc&X-Amz-Signature=6ef23e9f15f75b6528302daae8671618bf8847fc96343d141488110c61c7e729&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

