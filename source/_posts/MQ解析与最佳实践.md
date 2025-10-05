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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665RRA7IMS%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T080049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEeiP3gumFtjXvlMyjO%2F1eTk4aociK875HLXKeui6oUAiEA7y1kiazSD2L25qmpmHlc3S3JX4qD3toZwAzzLKVT0fIq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDJ8P5u58mcYrLCl6NCrcA4zPTLoyJOzeehlNgq%2FS6CPy1sqZQfGmtvTw2Femf%2FzjwSqgdZgJIUNuOfOLl4lYGoEOr0vpe%2FPneKAziP4EBbvSeOMABtMba1iGEzkPPoHq%2BjpUIxcaa5LFOuGgeJv79INbb8QeX9GxItvKf20%2FqoqrcLMPwdkTZYqbMIsy14zeKUfV8kDh3WOnUM1n9opDCY0r3h7qOhe75M8NwCl%2Bgj8JrWS%2Ftp1PM18dKqW9zgeX27lY0ILyH4qsMERkikEN%2FhK97oSV2YlZ59dgeTIvShQMdXZ8FbRuR3brdhfvn5zYkqidHyuMb0j1b%2FEBeJnEWwDyaPoMyt9qWh5bTj7R3m3onoBua%2BLGBUDkW7MSHdczc1aKRWixHEHOOmfE9ouUrZKbzNYDdC%2FzMVNVuoryFBQRBhYj0Lb1HzINkrlWA3LPVVVgw13VySGJc%2BCr8JYNg0auFw7GCyvVUSRoDx2we%2BK6zEVuDv1Nk1zJWZWfPYMMdi61kitvA6Vy2iO9fBLuOlJYs3DcDIpNGVc8p6O%2FbcQj569rs7qPSKTvbltZOrpj3oTE6zyfYaaTGXe%2F8Bfaacjs%2F4ch%2B%2BAd16Oe86RazVYg4vSa0zGunBOtexHtC0zbakaUZiFIFnjdlxINMLeHiMcGOqUB1DsdJ9vTQIiZ60d8wkJg6fT7T38EzeeHKkGyanT%2FKLlXlJFbLlUhMojhbPD8xHSX%2BMSPZ1BPteTWnVtPFtgkD7f3rkbT3hH1kyEbnsVCl0pB%2FK%2F0DS2Hs2eSjufMUGiCb5zOyh4qqsBymjqi4tOjIIT6QLhViBt4nqvb0rLdCJlMbuf8vBnvSYnQaJptcOrz7%2FwJ5NkQB9LrescKA6Q5z69hOEiy&X-Amz-Signature=01a5cd808e77ce6f688fe6f141bc4a337af9f317986794f0856c41fed4f2fb1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

