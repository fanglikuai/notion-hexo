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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R5EUPBU%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGvKslkBv2WhCdHwwvsVrd1VY6vQXpJrJe%2Fwq7gNhYbAIge%2BAWM3t9FGJTNUuFb%2FDFNVznBg6WZuSJGkPzJ7vaqecqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCmxtJsFs0Dbq0j9pyrcA2Y4kaX6WUedaoOKcbggh1%2F4uygOAnY9Xn0RGqAFcPTsDLpZnOcalIe%2FNXZIcGWEPEJ2JMpVFCl4vjNH0zD92RRedZQGYX2f8kRhC9UBVIpwlgnuFXxetJy35TPE2at2TJz8sFCh4wXrtwKbmy93iemp1CmLPMYKwFOSZ4NhFFNYUyDg%2FIu3swHjql7ypoWYt%2BZJME3xzh6SmYOu73Lx%2FYLP2M0MRyOzqBtJn9fKizrERNTBIYd3NwV5O7ABj0%2FDhueJp7W8JQZyqmRMkA2Xar3I0hbUelrNGsfkwcyExiSzTib89kBxNlduq0yo1RCIuxSvYUSpXa%2Fevj%2BteUddByuuZ0Rr%2FbuZXfMmvlyV%2FLSxnMMUtRsen5IkWBqhCFNaCVbCIbF8ZDvgrEp2zibCNOuP8T4Sy8MKFAvBAaxLWDlabtwI%2BKDqgwmHkmqJSf7MU4uY1A7J5uaVy%2BhxYWDRr3xfhRH6x%2BeTK2GlAyD6ICENmWi2IJVAKpyJCSgUvXlL0aydKYo9eVyduBMvxQKCcyoXLx3tJgMzmxxHLfFxpQ5wk6Rsry63IXcEAs%2FLhtM%2BTZEPEo%2FfzlAFpJ1uekjAO5nLhkaIvWDqU6Ez901D8J9YuxfkybVD0boptLVmMMzhwccGOqUBKiTVMO4GNFWnVyFVxhqcngH9tfn12ACGIYMsfInWA%2BhA2MmvIr3oBclBQPhSDFuS3GK3cjey7nszk2Rlj5ynyRI1Ln0twssJg3V5BhFNo9EdErP1yjZc7t2s2bUyW3wNCHi0qT%2Ba9IHHOfasaqZM3kWBcT5nYjgMz1S8zmSTcB6ai9vQftClSHSyOn0GV8oyi2MEJxg1q7DsPOzmqQosl6%2F5cHic&X-Amz-Signature=8244bb0e9cbb756b73640aafa9e851bbad84034ed45b7f17d1aa3d728ba389bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

