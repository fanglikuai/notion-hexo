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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3CZYFVH%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQDhuynwHjY7bQ6GFwwy333G8zSTMzZrb%2FW%2FwUZVEzwsngIgcU4WTo7TooaZ%2FlJAKfD0WwHFZ%2BUq9Clx37R5b8A7PCEqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGSmHzM7IyKdcoCKsSrcA88s6jkSJDh%2FtTj92CVqO5p7untDopRV2s34IsWZWg7TW3SIIfx8jPuyFsI0%2BBeNAAm29v0l%2BUebr6TKMXmTfhG8tecrGMXT3Tq%2BUojhz1U1ybgys4HIfMq1uvpJujdeE62CYok3XAQTxD8UHImYLTiGRqCjDyEGdkO03oSyYqqKC3Ew0KCx5zKo%2BNrQEQaoaqx5B6c4kAdY5vH%2FOzXK2A%2B0HWve7Pmgm4KoWhGUSQ38%2Bl%2BsaNc7RR7%2BQjkoPN5TYZgMoZuiPTpp3A6H0DBZqc%2F4RNozESqi0mgjFaXQDt5Eda6wztc9Rb3kcqhsambaArJBxivKAUKCLmdUD0AHCgVKGA0Zd7E9sA9sBntzdXJoNwCzMydgKdDBBHeLktRiAWPTakS%2BuVRbrFoWMCPyJDFW8MU0s%2FhhWYkRqzqpa9DhHvbVltUFzXJ2%2BcJKn92QWW%2BYBpHFebORg8sSOkRQ6KMwnRK4CE8rcMhS5veXjGASMoVwmqSrmuYMfSEqeSHi%2BM6xfbsGALwd6AS1Yc66YZAgpoVNDZItELiYUcaGaB0U8mIO0%2BUdw0Tquil5ruBKWG%2BB003PDSqcGFneX%2B2ByMrBKY%2BeYqzFVIE2Er7iU%2FmC9YrdxLzXAodXLJQfMOqLkcgGOqUB5dCJZFWmBXApG0t%2B%2FmrW1EH%2B%2FnflY33CjSiv7UBq9%2Bf72em1WZjdXgMKlDtdQKP9t2w1nLdTLfvQO1KiSFxO%2BiQ7ZHXP6Wnp9f4IzO7ANjSM6ocniEKbJWV6GPooL0cv6hZmxvLc0qlXFdlbi7uXOw2fgg69RL7fB3W52UQZHm914C5x8ufweXWA7pPqFW%2FjhI8o7I31nkaopacVs5Ah%2Bjp%2FEjis&X-Amz-Signature=fd4588061c334313b704440564656c730b9707ecb35c1c2fe630165c0727fe3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

