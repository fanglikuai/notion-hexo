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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6LPP77%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T110056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFhsyikuge3MHGgVyumxwTYaxedkjIZlCPsarIKwJ75pAiBgmDr9D8Hr0F%2FARnAcPkGpRiJq763q5i4G3Gv%2BwJbFiyr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMl49xGuKI5iVoLuY5KtwDWL2D5vGGxaQCCKHv5%2BUB9lxYgECgHAqvsBISO%2FP0FjHI7354z4XjGDfuUmZut%2BNyhR%2F4SX%2BmRUOj2M9wzEkPvhxdu1QCrZeVpDEGy61po1cuRsFSezu3%2B4JB1anZ4xPqjtoPXtAsTAnyrtmPhZcKALJ2lB2fvxhAEMl%2Fny7Z1ysfRZMMchUPdkR1lXCgEvPauHDo8whM98FurJPeKEHgmlJZvz4cUBKlwtL%2B91o5gd19NCX%2FQNLKcDcp%2B526VAYFpIjJ8DG9vhXcDKJ1as9a0Fs2hlVioVrIBpdtTCYuqc8Uv%2FD5r9Lc%2FnceUhjX3NtQlQ0YQNRO%2FlIYTvNb5ehmurioyCFfaw6riPvqJ6pbxkGuh0Xd3G9hsUuawDOxN2EHdlZDEd5joIOhuvYpZNfrLGwF%2Fed0R4uHCjADkX78FNP7xNy0OhNRlA%2FNc7%2FXl3SLSI1b0iMrfHDlC5chxTGYPHMu1wJdpVSFEONB7jqywVuVow2C49PjPKdEdmkaqugiPDgzDuF6jGsjZnuv7z4u%2Fotc22QKm6ji0jlXOc%2BwtmC4HMJkRYR9ezxeMCvrYKuULwIBhVbRFwBUlDzxO8zWkEr%2BbKA42Wg%2FIFp8x5aeckRK9uz4rugf42%2F2wtswqYKIxwY6pgGLhHxakt6kSiNQii4uXsimky0z%2BZj7AYbD2Cudh%2FGkvNW2%2BvzO4qdt0v%2BeH7X1wKLhf7MGORB909uM2cJeGYfO3IXz7OM6i4OJA9orvKNOOa2NR%2FW3qFsEh2xY0u8VIMXwtErS4ADdHC4vMnwHE%2BAlVs8T%2FPCgo2m1b355Dzog8Rb5h5ih0AkjbR%2Bb975KtIcxqvys56nrdsrW7OySUTZHRVWScsLX&X-Amz-Signature=ec27b94c6fa2e758e8bc0f0ce51c3aef52c86aaf70935bb875b3260610fb8aff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

