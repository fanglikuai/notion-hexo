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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHY6DEH%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCYMD2OAoTzKnewuGnwzseyZxppqEr4K3EMrqaqnBI4jwIhAKLm0PEjhKPJIaG2GuJT2Uz%2B9CoiqLkAidj%2BK3BgTmd%2BKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9hoZHMo6%2BQqCQcDUq3AOXs9Gv1QcZhLXnSoX%2FKEBfjOAcXUJzupn3K1CRCxMnzz9Y8SecQEoMTQnpJl2Jawc0nolNVZ2SeZdrdYzMYvjWqsSAVmPI5MjD23TJ4Al%2B4RZBlgee1lSqVqAKbQ15uQDV2TzgZwsWmcNjgplMJBvECkgxpqPAWkltlxqQ3bgfyR0af%2FUHr7Uijd2MaqP2A4P7OyPi2WIe1txCYDk1jyGcKhTt0yMFNyiC0CYxz3jaL9yKFHCUaNZu2eUOewMauzebhnByB5mLeTRG2XyNHsVqRpnUYDQwwWFRbsfCTiMzMWR5F1kyIG75xsqZ%2BrjCLyetrGjNLg1qDVHVkQxvHzG8XYskEkglLIBhf2EShXQJa%2FXrEHhmNljTTblub4UIgqd6yQPJrUGKdFC240VGxlSsgloXZvh1A%2FFb3%2F0HfSApgLdZsNRJt8l5NWB9nS9qrZlBuaHrwfsNyxAQeTP6vkZrXPWjHqdcRHnomOS%2BMA2cNgvi6p4OVtcuWJiJ0B05dYKehT%2Bd7xkUTHY0iCCUJPFi7h5sqXDRSGWJbB0vc%2F5hwZM8CtY7%2FM67dhe1w%2Bl%2BrZfLFTw979MpEX8J%2Bb3gNKb9qLdUJNkcRYX7PxtToYZVJNZ%2B%2Bmrlg1wNxnBjPDC0yM%2FHBjqkAamCiVqK%2BcQUtYLRAI7j9E0nlrhjvXTbV8hZxmBZNYTtFaSa0Aj2CRqsXLcmCbltOHQ3M%2B99GenHiehz8851mbwWUyW4cKTQugBfDi4ZaJYcwt%2FRtiNNB0lkt6DJrxdWBmsH1CQPClpCBkskevbgVs%2BsuEzaCgU6r1GGHHiOKVq9xUOOh0a4NoWZ%2BKmYyPyi3eczZ0tO7nNzW69nvSx0haGB6gi7&X-Amz-Signature=a96188b9b665f0e082696150c585f0114a39b03280376307e0ed5bcf14ef9c54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

