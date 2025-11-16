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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S3ZFIV26%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGknMPK78XRXByOrVcp6QFUo9L99jzOqKual0QNMELJAiEA7Vu8Qh%2FKHrBfBxS%2FErn1yKsatVVf43vQg7ErDJxbOLMqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGwUa9nkTQNtcEGe4ircA3iOJ5tTqZDKp832EnDF8ohM%2Bv6a02B3O%2F6Ki5dzmPfGcA6Sq3T8cSsq4VrzTEuYrnmxb2dZZHFdbnVedVbd4aPk8t6dD%2Fy67%2FHRTep6T54nLZsIF0qNSVhCSAUuiQ8tB31RH4ZVNifTTAb4IntvMVpMpnTbtGQK3w4bk1ZFa68ATgMC03sJSLX%2FMrj0f7ta1HGVTZ2aXqq37oEG8JYMoUJ2BpJp4rr0ImV4dFNkgjMUeY4XwvsMIhGu%2FpfXDsRaBsvwKpMJgxaGpjtOr7u2ZQpPS3YjQ69OiiT0oqnmp1RdiD46vrZD1eAk7Ej4Ylz%2Bw%2BY2TTr5EST9CbAdxSxKwMjdytx%2BIZYZ2ksF8QUXGPNgZiR2xGdffbew6YbzYEOvcWA3%2FeLyW6Y2hqrHa2Yx3YKcXSrKUyC95NAd4Rx%2FPjmaM%2FUvMHYPImHGiu7AlxyDmqmROM9wQFd3aCndOpcsJ1TtSguTyqu1Muyk%2Bxj%2BdINeBRZF56XTtAJHHCyBzxiR9eNkL8DUeOfFRx8wJBhbFumbNqWCfyO2jLyPjEM2WXfk9yB9zLRH4M5mNgSnqsKE14AGiZVC1lvdCrJiFElyFrEi9EEagWivYdOkCIgD%2F0j%2FuN542qiJncBVuNyvMPbg5cgGOqUBH2gHBCNwHlJ5SxrYo%2FE0vBv7jBxAlRTYONC23wr7AflpW8HwjxyCKbb5MrlQLJGoscA5l2xJ7Srgkf8YJgr0AdO7gP03QxBZgcvsrpE3adHH3KzP6XPbdDqao6KK8BYf5jwtjYWCMKeWwg1ZS1Sgi8XXrNHMMCY9%2F4G4xNCfvbmgdUROtsu46xHXzHNDd88OcX3VO5kP4j7afbMCtyFSJiRDHte7&X-Amz-Signature=1840c31e7049ba5e9e25552a050e2c510dc97cb30318316e5ab4baeb427e9434&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

