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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YGYGFNKE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGnkwloeSrw3EmIlZw6p%2FiGa4gMItg3Z3HudGw1FT0ihAiA7GTye11CdZ78pRjs3x0YaRCVG8PNEn7yO%2BnAJ5q81KSqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtWp92HYlnOiVBVbtKtwD0jpvCx6gJ6e3sk2qAssX8JvRMnnDF6%2BIStULBHzOl32er41FbFyVYUkca4HGqLWDGQCu%2FSh96d3CIMAYYilSoQiZX4W3iTXjXc3IeN8kJkk59xd%2BZ7GVpt0N%2FeLBSKhmzCZNZ3VVZ5PdSysog%2BULHg3%2F6AofINpdLcv3RbH7sN0o6FmRnGg2Z7858buPRTDNAmZzsfO%2FOEN4hPeCf3XldhAMjGjcxAUdpuX48jR%2BAwJIkOY4SE4wTADQc9wDupX%2Bxw%2FsNiJsOx0aR%2B11AKim5WczUbG0LU3bYCVYXr5gF6ahmO9bPUQSqzzcYPBHV5fQi9ool171ATKxrjVL%2BJI86JophzPElkX%2BuFSRq2ETtzB7oSazzCE%2B%2FgkIO1MsrUU2lB5C%2F1P%2B%2F3H0px7fiqFugItss3NaDJWNy4JvXNoWVFRs%2FE3nZ36ZwbEW4GWI%2F%2BIt4UjS%2BEc6b1ec23cHbKOdPNZecbUY%2FgPBpEfd44OtEdiYkpf6lxmpj7e1a3Ed929WE5Ig7glyVIbSnlzNS1E08pOusr6%2BPsfatwOfGwrky%2FyIbESNH5MxTp%2BGIfUNMV%2FWq59sChN8IQs0oTFmRZ3GYAdN5mX%2FvYszBYQgrvBjZ7FhPTsOhucipbDzJ0QwhID3xwY6pgFPUZUiZoILSRqi1tUWX%2F4LMZBVFxHXx2PxdtJntyn2ibTTO8gm4zzSrhAuR6UNuuurtmg%2BwiOIARh0hpkFZZXvpG8ZMg%2Be%2FASDclO1pms%2BXOcv%2F6hHAibFh1a05Ma7WzdtfQNx7TlVY7aL9ITkBbY3Eq0vEU3IhFLZUXk2g32IjClag%2BEIzkhWV%2B5idbIsjeI9yuAcg3JIcEfFdao0OKr6zJumkj8t&X-Amz-Signature=0ed375e204f52cfa3f152e534a7c6d72fdaf7d9700dcad5f0ab365b11654b6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

