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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRIRXYFJ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T060038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCn9NKKpOwJMxLr%2B5eXUbtmtrmOv%2FYC8q3pYuuR8EOCqAIhAP1gYZ%2F7bMzfc5cTN1XFsPUbZDvCAa2fTlZbkTWC9YhvKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwhJXkxQkFDVwiqf7Eq3AMpDdjAnginkMF5No69VzbslXIc48Q%2FVY6jWIjpcvlEkaGhvE2LHZbzfmUmzIQxuR8hTEE%2BUG2bwnlIgKAUUq5Y4nkFQ4wa3rZ4NEAxC%2FofWmwb4UUHl4WiAI90Yse%2FDOiu02knFqnRagYoBxobXA%2B1UD06Bn73%2B18P56k%2BT9mh%2BfvxGLqi%2BtQG6T0hIwdnDVakD5cdniWLl%2BtRQzns0zHoGrm6HUyJtde4PtBfYjG0wDuVGmQH5XsaFutZrJtv8koquR7EuxPV81liyK%2FVhDt0S%2BuQefJrSHDpEbMV%2FXSP75vW7wixZjBlKCdr4BBe9xqZ3Qwh%2Bb%2BnohiV2Cxt3LA4rBoclgegKWG6raGHKJdve%2Bynw%2Fv8afvg4DpGg8WxxZY1l3hVstqhcXGHKwECMxy%2Fu6kSxMmandwAMj%2Fc400ut4oLJY%2FexzyyCY9RuQ5gUtajKRp2KThtlYo3eyEJ7kcam0jmv7ksYXG7sQ5IBkvp6kkWCGSpnbm5tLipJt4tqbHKDm0lYbntE%2Bo07ccnVdNzVUk5u7%2B1Q%2B3S%2B9vI%2BSP%2BCxPboYH3mvoUxS9jOTW%2BCmbMoX28O6nI9vYvfI5Y4oDFGwfRGLDa9852MciS5nPjHxIg%2BpJi%2Bd%2F21Z%2FdnTCp7vLGBjqkAegtjYxKeYJ0D6O2RZKh2%2FXOZCoVbKYHot%2B4%2Fb0KiRgEXnADp7dFa4pyetC5EfwMKnM%2FuizMcEJuAy2gvrGcP4w6A1A%2FMrCPTlSK3l0qjAT7EcCrKHtSpSj65WJ7UvPilBccQkKpAnOLkcvFB73lXTneG9AVvogle6xUJ54pMtq0Oip3XKzTwKh%2B1CQkBSX7ftGfQXd2HawqUYa1j%2BUy5BI%2BngFi&X-Amz-Signature=fe595944413a294db53c6dc5fed51776c8703ae9f90dc01452f3d5d90b6fdcd1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

