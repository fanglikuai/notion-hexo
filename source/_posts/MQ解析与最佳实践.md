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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XEJ2QJTL%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3L7%2B71515DZxueFKLhNE8F0ROeGXXwZh%2FC2MG0NveNwIgKTXwF8Kj1wBP8FQyei1zZSiUDQpRYzcgjaYxlmGumYkqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL7jnbKe9m4cCWsNEyrcA%2B3W4OVrbr%2FzirVjweOZpMNbDz2madc8aZEKX5LsANC6wpLd1o9uMHZAdPa4nHsWrB5%2Bz3tNLxAXntwuX%2FBJ46KPc7dgXsLbMS%2Bb2yrYHfhPWCtEisH%2FMjnda7okaDY%2B4A77BKaRTpQWq04M7R2mSgB4PTUSzAiO5cvgS%2Fm1uSSrxhAt3%2FZzXbJqXhbaqJZSq%2Bp6ysGeGcTaK0ti0PXH9FXeaX8fSFzR1GqeLONqNJqP2luSMlPmiUlstb83Ww3u2sHEyGaQePEPvddSn4jbXgs9HmuU8iYybJpTWf6T0ppKnPfMSIyjOs%2BeO1sK3mXSNbJpH80V876gZ%2Bxm%2Bbu8KgEISUfimgZRdGNTwCD8zxECk4FJHIew6lYBq9SXZ15NXc3%2FBSxIxHfzF7N8ONkcbZzHFb%2BKXk0KqUpYmtrl7W33V0y7DlxeO34IpJaIYCqgv0GfNcGYiZOpo8zcXuseouftCoJzeBUhPEFDixROfrdE7FaWn9hF4WoT1q4gYSxoMutyNS0KGY5OtCas7l%2FjuJb3d0%2FpROwdKk7qxcRwuG6mLvuiwWEHINzlhSW3cmO88akcK71VINlfODIidnI0RtjYV3ISQyFIu0Ur8xFSozfJFD0nKIGALgL%2FHf2fMNSFrMgGOqUB9yTTAEQEwBpob9pMmWuuuHRTOPpneXgdbU120xrU%2FYeLTebcQ8V4GdZ%2Fj%2F2wLSADP5gSGeuZUQkHRUb73j4fuLqVBpEnL7wovXxoxuvqsPwcakMamDeTHEGNoEVANX7f%2FS%2FXHi1ajM2XjlZVDmV2IH7bJKNHHsrZphhVGPStpDfnt%2FY4qh%2BomXI3ziuxq4%2FxSsw52ZTAHY%2BfTLGjn6tonGJmQY6u&X-Amz-Signature=600a1c6a8aea60c1d7179f04ec29f888203240c0496b554ed0a706129094c6e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

