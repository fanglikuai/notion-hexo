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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CL6VZYZ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICRe%2BHRFklbaHxIfnGdJCGxh0NUWI8Y4QPJHIvxV8a6EAiEAnSD%2BUc9auSfwUSqYDBaClDTwkrXpuhZGluAHrt1rnKIq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDLAhI9pY31hmgPJy4ircA3aaVI9VkvLsiPY%2B3MK0h%2Bt%2B%2BONunnDL2tC%2BMoj0RIHZgJVHw8WeZsTgi5ifKE1yAToVz6%2BvKIL3yficvlEN0xRN8VA2WTVN%2FRNaa59u4TxvLM5BlSdM2GvzpSTSCL7Wd17vFgp9tHuPsUaYvlAsxC56MLWmtzDhApTUWfe4MnAOsET1WbJaPDNjuMpIjowv9YYB1vhXoONUPgfJf5gEj42qn5phHcWlLDxrNCAdP9C0MBjJUo5RmmCmJ7Dv8ITzGcfvdakaudtNklmv0kqkFrdb%2Bsl6JheXFlTbYdaCW%2BA%2Bpjvj6tHlXcq6%2Bj9KYx52lX7WJFan135BIi6EyMSnJlzBE%2FDNX3CSpXAswZj7KTkHWInaq%2BtscW1R3FBbo7wUVhcI1oNZFxWKX3ACJSTmkkbvm2qwuTAKHIEuh2WiE%2FRQkE%2Bupc9OeBzJOcUbBojFh1Giz%2BjI65jCdA2kwwZWWiVK0yDj9K%2BvBZE9mymmJNa8UCEzZVLHoiF%2BfOGrjmLehzre%2B%2B2%2B1iHuWFklAMSGtvc%2Bs2wMVF0eseSwWXWP1pPSpiPgqKHWkRKpPvMsK81KhIjet1LGE0uae3VqxXw9iuXChZaYaxND7313OXXlJF5PwlvdBmQ6IPpMNYZLMPOEv8cGOqUBwpHrYDnPDGv61%2BobvSOQ49I%2FVtKKkHUNAKkbKVKTQb7cX7MLNTqgWdE3RO7T60bo8%2BThGuyEhDIxeWG%2B4DOersRkS09hL41dqQxvpvON0J0u6%2FScCycaxGC5tb3chdoWBvxb5uIzT%2FltuGUdmndGB%2BTfF5N5eEKfYLVOMfiitT37nzo7mE3G7AkM6VO%2Fz518zSNfgLYM4snCdz%2FZchdHJc3URmGz&X-Amz-Signature=c935dcabb12ae6438efb5eb26f72074e1336bf2636d32f1993a5f0c6085a0424&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

