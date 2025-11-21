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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSVKGBNV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDwm99TOpEzHuLySKJStdD3UxTzVGWU8xv%2BVGOSJM7ISgIhAODTw3zAXMu3HPN4tCzFMsCkZmKZfOiyI4Lzb5oykpiqKv8DCAcQABoMNjM3NDIzMTgzODA1IgwtlII96tRo%2BLw5CBUq3AMF6HgQlq7khBIAYpjE97Zro7LFzb9ePQ3YAReGQOmcQX95L4V5X8xndKbJy3P58PidzwV%2BoPNVhujZRsYkSdpYk6F9E6nKllhrmA8lSpohDkcwAFVxkAOgyvRt%2FIbNMKLyJABqIQFEM4o9auJUZKg1IoRZhTYnSbF3UEXZK5shjx87qtoPSPFB%2FYEVPt6ZTG6X%2BqyLo00VQQ%2FbcA%2FQWa%2FZGwsfbVpeN%2BYm5y9NL6GlVVcvQR9eCy%2B3nNqpsiWmrC0A6NdKTZvmTa%2FNmxiThmUp9UAbbiNAwVbbBJNOrEgP1VnaibArMPxrbSn32dUZ9hz46p6ZiepYD2ExsLfhm71cjMjOBSf%2BIdJ1oF8TNCrAykA17iwr43K6Fku37NreagC72pe%2BJTtB0UJHxcKeiC6ixT2t%2Ffz6QQGZbNgmf5ygVyyh9JbhgrvQngNnse7RAr1MvRRpdbz2dxqoR7fp%2BrHWWRha73Y%2FOFZdC%2BsbNEEjgq5GP5USfWEXVl0M%2F49%2FFR5ApHbvaRJPta8Sxn%2BLXMRqd38xcxWRfAVdKiXrp%2FG3J2oxI3mVZmkHUE2Sf6zCd8uqf8KmLRf9cDx4KeX5%2FQlAyd%2FYh76%2Fn1VU6EmGZmXhSWPGcpb5F%2F6ljDevZjDVgoDJBjqkAUGpnLh1VNQjcDgnrd85Wh%2Bd6eGxk%2FS52vlyk6LGyOkZfjs8wNOgn5jt6wu69F6EY7gS8n56ueto9eDw6tyf9KwMoGv0e8NqaGnt7XB3qhL61Yga44KNndMtYg4k4Uv%2BDpMzkyJp8%2FWbr6mOrHzRMwvGeGF%2B%2FS%2Bau5vKmU0lc4fkFmQtFaJ5XTxngkI6E7s3mH2pFTABIvuPwriG5ZP42w6cp4dw&X-Amz-Signature=ba80479cab8aa2fd9e078d0c8f05d6dddca285fbf855b8fab3c15e53baea82e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

