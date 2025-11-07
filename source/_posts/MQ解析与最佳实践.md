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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673BQDHJ5%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3npDN6epBgnIvEkDqQrapMxg%2F609B8kBIw1VLyRRqZAIgGNAGXUGGPoiFA81PnrlqWDdj1wugOpsX%2BeVuzy8aCRUqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIFwFU%2FRfzME%2Bdr6jCrcA23c6YtiBlJZd1rTMouvSzmWyc5wDPAL3jijWGhFFDusHDjnRe4AIp9AfWljPoOmnfOHbPyS5B5T8LRr3gFuFBx1HGolce%2Fsca2hy1VkCaLPlasYSTGEo8ZWz8RHafrsAcygFMFC%2B9sQGpbgYV9Lzm1GXMhvupNC7b4tMCrLF6%2FCvkc2m6zo%2FKBkb5YDXSS3tiwnD0fTsT8dn18DQw1%2FNwOImXPNWkoflvsX98GE2WC9qkrDeTDa77KGx2bPK%2FtHYLYI633ntKnLDHQmHUhW%2FH76sclaCr4I2XBthYUeXh0CGZzp77KZEnrKd7%2BLVg7qnyFK0l7x7PbFK7a3Mpm50lGsvmasdpqonEopMqsvWpY7qbemSkr9Uf6L9LjfrmZ9EF7fT5t0Ave52lZxtc4hZqvXL%2FKGIj8JYKQ4HMVX7hCuIiX6dU2RJSA%2FA2vSu3eMcWiTafYZP3aZPSF9UG05JpnIr9sDUPjdKB6VT3biJ0KImuEu8MlpSiAF811RA9BRPcREuVwDojyjxsD3ZV%2BOCXYEom%2B9q5gmmoR1kSoMg%2FK3Qr99STwjPYbJIvj5583UI03ikSSZaIr6nKoF54Y3lKP0%2BYAnDjoPExCXIaf1%2FAw%2FNotnTHGeT25pGW2lMKX9uMgGOqUB8%2F68T5qclWUJWc%2FKm4DyLUPv3ar8HddebZ85KpQg24KejR9WFKfs%2F9kYFtCUrSlzs2rbCZLv6aKZ8OYc%2BUNwYEElSc%2F1KP0z%2FWoL5gnneFFKlYvGelFzxa%2BD5EIuiAFdaN5QqNUmYo5oSRjgcpMZo02appm8DA0pcqlaK%2FelDc%2B1sLaHZMCaiwQBOHEaJfrLg%2FbTywaMW%2Fn9ZXi5O5ih8PpoMQCw&X-Amz-Signature=f1996e9515e9e70f771aef166a217a376322743a14895a971d273c1896fb5b6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

