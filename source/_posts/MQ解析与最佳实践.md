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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OGITHNK%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIANyA1yRJ68WUNt1LMap5G48bmFgLkjmgsWGed5BvEm9AiBQZNcr0fSi2ADmHzuJFEflSgBSAcnqzKXPWKQ1hCLktyqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhI6rjEV4dND0qVu4KtwDLM88VCkqavVtBWKHF7KW38w6HTDJ1R4rGw1IWPn060G9NjuH4aFTiBI386zDE4m5w6uDdmUYOM2TAPfOpxcRG83ImjDozD1g1t%2BJbbb%2F07Jk16IUtD7X1nLmoOcqBhH0UaT7mJbXk6oh7I3e2Qe4lyYd%2FUzTu2FgYGIL8NA%2BDOP8uTooMEJof9gvVBL6pgXQifc8DmyqRS6LApJsRuaiukC5phPr1MNa7JGH9FF2x%2Bd7qZRmAEERQRrZWIiEI6%2B8W5ec8Cw%2FX0wMUBGMu7Onf0c%2BHpQM3JxrjP%2Fd9%2FBby0WTZO%2FpDdfE9knEGIfrPw8Y%2Bk2UTRt7Tf4ctD89wI7IgqIvX21ZaWf75lSWMQI5IZUAKx5gfcc%2Fv7U95XiJT610oVsNXe28vGVhnlnYxkyZe1QVZSsgpEtQeaE%2FIEYXJovHSPhDrqgNRghAcNxjQUiWHRleoJlnmbCVKPlmH6OnKaYX4usbxPBpEjs488LxBLIC3HjfaDpEfNLhIIOff9bTK3RHZB3tAkb2G6k0MxrJz7JQBkmBnK8NxWXVuz9OZjJatlJjiz4OunWjI8jUhQ9luY5DGPJ%2F6umT3EQ0vHU6OTAW5xJA2En3wUoE%2Fmv1Pdz3o2GfjDfOkboTLHUwnZb2yAY6pgECTIC3WaiwPD5HLb3NWrGrRo%2Fdcf2hc9KDSixP8kY7q4yFJj1Mvs10QdXsyIX0vYrIP2PK2HXi7ZBRrUJWGBk1JwMvhTeYKpk5lAJeHEmIkqcZ4TRR0lympKlzjLCnvAPdWJjnlh8mFR4k2PN84IaOmS67qvXz8kcQDJGBrLO5caABqqyMGdoKtf1TG3htP2vrsMXesV8%2BrOHXt7IIwYAfTQN2ODYR&X-Amz-Signature=0a72d7360593da164686c69fc60b3f4e72c9d98a9884fb0c7dc76d0249575441&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

