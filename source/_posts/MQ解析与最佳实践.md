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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBOKEQ5B%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQDJYsfgoL1sPnRa2Ks7J90TqWSYOm0cV8RXg4vgJYJKbwIhAOgw5h0cNhUDSgZUHZlH3%2FUjneWGehu7GyJbhd656iMCKv8DCEAQABoMNjM3NDIzMTgzODA1IgxDBrMGS%2FLGdBnrQwYq3ANaYfhTh0hlEbOCZw8e0mq07RBjwaOLM9YjWI6GBC50YAjuqeFTcQK2rKDLqfShxuaZDkrYrX3SXH7uLSkkN52Acz1fRxxX%2BFzexzB2DBmEMCWdiFxeB5YzyxjpEchFX%2B3fVq9Ne2okgFV1P3DC4QsDmI7vu4%2Bk0o4f1CzkYqCeQhJcxA5omY6aGB%2FwPihQh%2FLstLVAtZ79%2F74uNx0ITcMTs3lRMrbvogsiFw5tlgtC03%2FU7cMieVuhHhes6iSGjUJ%2BEU6lpqtamMO3jnnAMdY3UqCsnlKfILxLduLXxro4jwQ%2B8Pb3bG%2B%2B8Sfu5dFDoen0I5d9l8WVyGGUxgWR9g6Nx0nH%2BGkUA%2BgH37l3nI42NPjvm2dlQOWTksbt6%2B8GPnBDO7vy3fGzMsNAHdBRGDLrgVUSZBSAonFj8GPZxl2Xt9%2FdXJzIHm8kZ91hNccT%2BpndyUfclkMdEwjGJAHrxH6JEn4LB8ksVQAqRDqqG9wi1M3P5ablyMSdx7WL9e73HakqDrql3Icva5Xnp%2Bve%2BjpcuNb1P%2FiFdyJQiKBdpDUcou3C4%2BhcV9ehqhT7yar8bsNjp4%2FIPh9MF%2BG%2F4croD1f7G5kKP8m0dEPvxTYLrO7fl3vdPowLT9u5ijnhBDCOnNTIBjqkASfGhgX4CSpf%2BPZX7RiH1%2FTCPqkMiVydA2%2FXGMAWtME3XEINs63nGC7bYJJWjeBwvbKrTx9ezfgIgpPFwtTKKpqc8GwLOGs0RkOyMAvPv%2FRm%2BWZydIPdkeav6jZ2b8Oj6NX6hKeUO%2Bj%2BnWDWXPyLmz1gyXK%2FEE%2BlBtrimPBaFvV8T%2BHMC8qFf7YP1tSY6PZcVZaawzrrZzxqVS28c3AB534Ndc48&X-Amz-Signature=dc44226d44f21b304630fe04918662c41658512ea5b61ea6b5d679a75657e25d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

