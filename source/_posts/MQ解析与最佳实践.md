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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI5VXCNC%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQCgN7%2FQjDJLRC8N3OjtWwI%2F8FYqiCfJoJCAOYiIXaQlSQIgH6Hr3isyak2KBo89lIl8dFjTPvux0%2BPzgBFr2t94pkoqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYcOLBVYj9Gf2yn9ircA2dZm90UrS5wIUx5KAjZ9pU%2BeDllQA3XA%2BUpRq7v8XdpZBJgd7fFYR0PBeiW1CYdRmBQjo4ik%2FJw5zz0%2BGNhCHcBC2D%2FfV2Oce%2BMxFL%2BJnFW8pOHHYzCj7Cw3nR9DPu7mCW6uCahvS7i4oknzRDVFlIn8J88A5nbqS94LW2A7k52jkpBgjdoEjhYizTNOU5LBxsp4Eg3QoOqEYC67kY4Sz4l69hki4DJro5IaWo%2F8PNVQCqiPllmqkhugt90zCzQAoki%2B8xbJNAHouTWn3gO8E8YbAzUbkVTBICCmSOFazz33C985Wpm9v%2BEGdzW%2BbMFAutf5y3dcLzAdJ%2FT3MFFEqHlHwQOjX9B%2F6h%2FS88blvMkrEu8gr2zgLcN4BYsu%2FVnIa1eH0IDjyigAcPanpNqDS1m5%2FNCuTYrULTKBUQsHjQlKD4XFaRgV%2Bo%2B%2FDB3NTtCXdR9BTFUQTSnH5c30Hn3GZT80m8gMzznVzApXNoFqPDuD5cNNCSQSLRHpYnv1GFXQxFqO%2FEEFByWRPJfGp1Ma2jRYkoo1jINXR957P78wqr24TW6UEHXGUnAFw1Ia5alo218g54UD3ykGtpPd1EeOD9dx%2B6wDwGEM%2FKP5MsJJrWECKrEu%2BHGqEjwcZaHMO%2F5%2BMgGOqUBT2gPTGHDZfukkuOd2WzzUC0todX8hCJClVFly%2BRFxHC%2BNTFymQT3BelahBKI4BWowqWSJab0ths%2Fj68T48F9xC7xPuxhj%2Bqx%2BFMGvSLQAN1ponUPSvpjUhKJfSt7UEmNhOjT7xJu6MIm5exSH2Kkac8gnLRYIXpUDE3%2BaxnjkxtlllzdS97CDUNovQbmdMpm8%2B%2B1cy6Wh5NUNJuvMYxibayJz5BU&X-Amz-Signature=a394572947095508fdd311f761a9967b646eaac70d4f13f31229a22b79c8226b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

