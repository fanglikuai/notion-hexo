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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UV5R22JW%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEjwjjjZFShYbcxqtNHF1fiC71KdlEmVtHtZ8t%2F04E3AIgKkp2RsNKHkNPa%2FuJVy%2FUBgrb2LHjCu0lTvpTvjouulEq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDE2JmzA8NPQQMGvj4yrcA9VLtkumXYd5pWQpt3wTaWaNO%2FoPFUNiVvtxaLPbfWSUO%2Bwm65PiXi1LkMxV889dqQY0w4naTPApVz4UpyynfqFGsR0bjPfyBHRyBjOzeh9VE%2BMsi4fVFtCnkx86AL1zaq2%2BUePZ%2BCACBl80mnrwjK9UR1Xnadx%2FRnyaaayO124we5M5iNF%2B6mJp%2B3aoU89kPtf7mBM3KOCiC0cp%2BRXkdTLE449KTdSBSmIve2724VY7GQqQs%2FxJ0XKN3mHEC6J3mvA6Hrxvj75PfwBcfxTNfQHiQgIL7z7ESEVcWu%2FWjEkXyMOundAnyCSIwdhwtrpK4M%2Bw5g9I4qi2GzQ8EnC1DkBNsYk7qv9hozjy2JGgoTaV1HUiQg1VEMCpu34dH82QvNrxgkEUwR%2Fz9Fc%2BdEpqZ%2Fw1h56SSFEvWzGM1i2DQWEHWEI5Ah3mqqjvQ4NOSRjNh%2FRmQ0yfanuC%2FRfycsm5WRC6zcRwaqCRFhj5BdVvQoXu64XoAbkTDCHzY1N9EClckxQc19j%2FG%2F2%2BHBajmgKBl5ijBN%2B6B3OjiAotJAc6ENmeapg2lugX4wSVXBfkw2D9WmjUK6t%2F6Hrlc%2BCLkjUJdJ%2B1A4nIMGsmQ1WSFyNemeBxGRJi8i1Rddou0ImOMM6JvMcGOqUBUelGcn%2B1wY8yryarkqv%2Fl%2FGPol9sJUtP3bOtdxjpaeLS0Bc%2F1VplE4CJLNLlIE0Lb666%2Bz%2B7OX0gTcMCZR4RWlv0mMZqpIcjq%2Fk6YQzUw0wN5V4MeGx7pa8st7QmihlTjj3MoxwXyZJIVaa2Q2qQ3uqi4k968Utwl8QNY0l3pplhrkRHYv10J5vbJcszVYeZwiDsh02zl4PsNHj2ugAmwtSJT1%2Fa&X-Amz-Signature=d3b1aa5a4ab9320df0b6c0aac2e40cb095411f2a675ad18201ac724b6646c387&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

