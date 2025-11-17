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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWBVD5OB%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBfVoPJ3QVeQ5cLv81d1cT2cYgLrVD9I%2BoKGYEtSTMWNAiEAvJ3B%2FH262brw1mLPy2Uz84%2FJR9QqWPiEdFkO1QsM0aEqiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDECaz5Upay1jv8MnGyrcA%2BDXmzFXC8NuCSFavyvahkaJhvHltB0a2yEQqqNwFgMxrE%2F3b6DD6xW9PmlE5KPfiaJC%2BQP%2BIlTiPuW%2FcLLxs7I7O9WMAWLRTuU17fBsAuVH%2Fz2WwQWROs26Bj%2FCQ0XqwUJLFyliL7Nskp%2FLL00HfUYmSVf3YFiNxb0V304F02DM3EMXA%2BZcWKUbq6uyzk4Pov8iYFNqnT9O9hBsdsAfOFqgr2zeKLNPxWMyAMHt1df0DB8Q5UT6fKzTH0UeVpe1F%2FSVYknllmJJQDRAzT%2F7%2BAsDItm3gH8cJJQZgURMxKk7KD1fs8Dk5G%2B7h2cUnvfSTSsapolYo25bAiVAWZusvm%2Fbs9qTnmO9HwwiAYxnMtnNeVUj3KbC45deEXWqoojhUAgSrDOE997LFjV%2FvYEa%2BUp7gRTLbnYX7O0IUEkWMVMcHqxVxqTwNwKTn76QJWDyJIWhfck4K1lIzjmJ9GeGcjclo7BPCdsRjPjSqFmXOu65n9m29pCIcc2IDyH1%2Fpe%2FI%2FNwJK6cKATzTQfpbtPqAR62MUpCXMUqyGVOQiiaEU5xqU7ATxzoSl%2B5%2B5nhKY%2BK7PMZa5z5uA5h1l72%2F30BqLxtEvK%2FUonjnJzyvTmDLBzMImMCVbhamNVhCXpYMOrJ7MgGOqUBRghCzS9OjpqRDBkIW%2Fcfz3cicSnvIv034QO44b1ABrYqX%2B9003PgYptGRSJ2gxcp3fzENzf5ByaGDn%2Fjc4xz6B%2FraVSyDYN8%2BgHN3Ug%2FABUIDrnP0CmUmZXIjb740droiDKlOg591s4ND0QbbobclAIPq%2BoqZgWkBnT2evga0abHL4v0LWDn0PqWqT03MV%2BBiVn4hmmf4HWV4qGNcj%2Bpshj%2F2Wbq&X-Amz-Signature=9078642697ddbab3bfe21eea68483887751e8a426ffb94ec7ec2805ce1fa11a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

