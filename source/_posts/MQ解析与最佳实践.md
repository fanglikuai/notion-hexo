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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ULHHILK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQC3NLLIX17S9kj129iUkATWdMw8yH%2B1h7ZhAs%2F3Kt%2BJbgIgcTUJqBGZXZIKIIjh0v%2FR76a6vMVP%2FMOPH5Ho129N1nwqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDN95T42TwI7mpntV%2FyrcAyzz55Qs7d8T2MKFPDE%2FtuT1zR6oZvDpTfPBWRKNPejlBzaKiFi2hugsVO3iK6P%2FR7%2FNW4gjNdyiYK0TG403b48GylSJUabUHKH%2FBjk%2FhSbytQNiIXj%2BxFahTzWCITMyBhFCsZ00LbT78dA8%2BKcLWKXkA3V88KzY0zzTk3Tan43nJOn8uK%2BNbwU6v0vCQ357u5cEEPd0wHA17%2B%2BgEEi%2FWKD79Y5NAFo8JzM3SIDVjaq3VU4Wh7Tjsir5CYr5kSQOfGyoTOdwHcmVn049t0XNPp5gYSTAWLNiaylbSXuCCa49bcYc9hVZ1lNa6qukWWSPY%2BaJNjOCR6EZa1yO8u0m3P15IEf%2BSTa1mebQcMOdWkvVu%2Frmud2Y63nJFdqJsN1XOXWRgHR2J0G9ifr0cHFBQjeGIf%2FO6nfOn1khO2FTQr6gG3PXglGI6Go6Kh46qv9u6wkwJ945gSa3ugg%2Bkt6oU9g%2FzUVn%2Fii5WYnnh2QxzSrW1p2AIf%2FK4xlQ2a6zg%2F%2BXul824hs7rsKMX%2BPDf%2BWl%2BedhRYptYW8dV1rnZdTP0MF45k3z9a%2BQMNJ23V%2FjhgXQawWvjA1GXVVD%2FMRjM3ysK0tIjZzbZeEs2B3DUvA8JUv9ld3UwoxgX6tjkzxQMJfqnccGOqUBERHo6fYa%2BUTOqvOUc0UqWHkerl1002tK8a8VuXa4q20KY1CgMtpPX8%2BVzGfbLZ%2B21EA7sHTSOqBA2fkQ6I2vZoJ0d8bK8%2FuewNEB895vCW9i%2BEYyGsKAQ5p9tMM8mEQsYrcM1haIKE37PuY%2FsrFpf7IB2qozCSo8rtkcT4a7Ok6b1kfXuf922ExcAiRp%2BD1dRd4dsU7Tth%2FWHlMoqRar%2B5md6I6V&X-Amz-Signature=ee96c8317f9871da20345b1b7b0b52031b17ad10e9f55214a708a5ec2267b498&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

