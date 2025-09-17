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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNCZADRK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIGHApIZqHYhQRUd5VnaVBtY%2F98Hcc3MS4bAPPgg9f%2Be6AiAo%2FyhkTtKGbiyqwsyqUbSDq%2FUxIz4LhQTdsgyg9T5gJCqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXGJ5xFPBiv6Jegr2KtwD%2F6JSuX1jtKbvFyj79F0lYcOy9kx5XdSm3%2FzeUm1QxXNFwFMQHLCargYcSYkkFNSYcE7s2NKOa2dTQrk9i8GbK12ceIkS8R5UE8tkH3Aivq%2FXAiicCA2UUG127qKPbKgpKiIG%2BgAK92hhSjG9eapOf41rPmOYwMSgoQMy0C90gMiLjARFccgY28YXk%2BV4PEyUnuFzht03Zs%2B1J1oJBmwjmRxtyA3Rp4xttGGNEyFXayOP8B2ptDfPpZnmtsgoFwqQ1ajaP7VKO46PBa46qtifM%2Bi16ymiVIWioGFKU3ShMpGN2nda5UG%2BmjmjKPmAzxHHoAQ3SCzTYhTkJQyNhNxOSh7WU2IC2aWgIVNZNMGnC3oXJxeFAJG1Dc3G3rmR5J0%2BpduClGcKjtvssKxmnYXxUCsiQYoSPH2L0dHutyvQ7eqKrXfwgKQMxCNUG%2BmBijEkTo52FwxsjFHi5irVmQbgm%2BJAQKb4S%2BUFZGOlwVi8hDfEE37ong5cL1XxcDqzsx%2Bunw6esA3CQBzqWoA9tM9UajAALTIw%2FsFkkSb4X2fCW%2FVTSNfNQvSctgT5ZKDPxLxVvFZuRzEvb2Tk0xhf7VVW5RkvzcFMLWEBgZBEi16E%2BdxDFih4G%2FYcWzJBO04wnqyrxgY6pgHl5h19K829lH4NmrqmuPE40HumPteLdlJJQP69UOlBWoDh63pYsK8o%2BagyUAm2Ri7YJu1m1GlUcOnCXU2X%2B0yOjs7RHMyhSv0EPeZzFsNWBRFzu33eA2JJjP9p%2B4JE3zT0XBGL9uE%2FOYMYHd5957EbdHQ2Y8mb3ICxXTFB5ocksPr8DvAQMw5ACSL99NCbcCdEp6ISOI%2FvDsfKJSHLprQ9bwDC5OJx&X-Amz-Signature=7972cbe96e3d88b4c27e7cc08fd75ca5d307332cb68a451f102c4a818b3f22c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

