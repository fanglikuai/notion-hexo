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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WS6RAHFU%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIEueimI6nGAncmOozpDN7QLa2OGGc%2B%2FFQm1PC6RwxhwMAiAYSi3%2FhH7cvCw3L3%2Bzx2A%2BpyguoxK00OBtFfgG9DCe5iqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv0ITxOmPfuBuB2P4KtwDuWQcxhb7sBitMgkb0F0IsdE46Mby1cQBJCGz4LY4kpKBlHykIp6HTzbDR1kePjiKRrelG%2BH1x61eu0pfe6wAkGlAUeqW2kn0DxKInK1B5%2B5mDyKyOfnOumZjgS8oXBO2Gyw9RNla0AwmWLnGsaBHcxrfSNsuYVuMEe47Uy1H6Fb4SK298awUuSceCbJcAPDEVb6bZnBUIVMDJNoxXfpt0vwbaZ16Xbylv%2FvmI0%2BW6AVFWcy7tFVLbvXgkPNGGXj9xwV02WUvUmw9NjWt39bKD%2FcsnkrUUhgn1V6h4VR64yOa6K1K8Vg%2FvDXKTFnAhatGow5maBgaO6UcjdV1ts7dT%2F7umq75x5zcZjWmYn60ZnVHneVSan059ZghWq0356kHoZ5CPfc9%2FxbrNTsdv7vMnCRxGOHlq3ltwhaFyFdfw9z6v1xRwOP7bNYxO%2Fo3QgpFY2qYvYuSS3JOoDVSQPICYRiOjHQEUhZsQE8uhLb0NzrxiUmR%2FtRwNlsS%2BJ7JU7uXVjqgOKI80hy%2FCLjJEGgLhZ%2Bei75E0pyi1jl8BGWSJnKKpORcTyPnv6w2QsZup2ZNE2gU2uJoxB3LQbtfvDR9hoNKfg9haOQCgdHaD9ZXqoNmzPGSyIOO8DDp2Ncws9%2BKyAY6pgG%2FjXqwqX%2BO3IeMQ7Ba9mUne2tKIpWT52ZaeBxQliXjNPkyuzEZyWLCCC5L6q31Ul6dI3%2FNSUaUQsJmCtHAcbXZImRDrgq%2BG%2FKlv82TK0Rgmg8RlTwWnhrSJWDQJtIdBjBaUecxJ1cnZyEh9A9QGpCJ83ArRGwOwHHP6DlChayFdpi58wQp0P6yo%2FiO3WF6it%2FTdzPQDxKkwc6iGbmhJvV%2F0bmhhlvi&X-Amz-Signature=9fabf7f9a83c825bfee5636121bafa41baa9b21399c36c5648774cc52671e6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

