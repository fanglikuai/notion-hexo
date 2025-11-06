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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663R7KSK6O%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAT5BzFmt86l2nKB9rl36KlrU727EPez29agOkCLQ65lAiA6oY2KJtFltBjnkvl1BeZT62bVMecwa4Xot7MPFp2wJSqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYSbpQi9VROah1xolKtwD%2BWDYQnmRKE4FJLBcTBbXVQkePrs7eJiKqA%2F%2B8vm8pTiNr0EA7LM9V6fdQca4UDNZwpyBhsLGlElbOGD0Qzt%2BRlIJTAKNA1XjXFuWgWli4Ysk%2FkvISiUeXKOEHWeu%2BgRSR6FWc%2F%2F80%2BtWlPRUzS%2FUX1krCRxxIThHPf78xkjhrSyaZV58xkmhnVFteH2doIUTMAhMbieuwWobAw2JzodFht6SdXjljiFGNTFsccjNvZYXDKex9dS9P5raoYolIzRtdDwgREEEbZ0V0YH7NzhLIYT3kzWyvpLERnt6LWE8IP08othBKzFSh4cf6EKoHxXqakDtl0FNjluT1g%2FGp1yOuktzGUWaY%2BmUIKjWAuMmYjt11KvT%2FnrY4dqYMwkQvpyFAQTC5bS9BUGixjS5VjeD6Sw28WtJ4usOa6ylgjV3FNckMpD%2FMc0ZCUUvTobIZF8rEenEa2wVoqZfUB4%2FKLgMGtOHNoKU66MyOdrRXgEQKnQsCGS4e8BZ3OEWuFBdjWq4I0xiUpNa%2B6JPQ6XINFOPYB8uwNR84V5d1vVMq4nc60Dg7uC1ZthqtBaexXKc2sdmwj9klCtvz0uX690Y76%2FIDhhjl1W5MsD%2BWyOnDuDC%2FYytSFu7aaePub2XDJIwlYW0yAY6pgHZOh90EvvBpOmh%2F7dU94AuduNKgJOyie5jyidgN9NLl2iwTGHNxEx4A0t5BDhU%2FMhcoYNCYu7x5yleJ3IX93mxu9%2F%2FzfyMWllS6R0tudrI8FLfKvakNVMPbf%2FRdgoecxrIlXEpO1SD4Bcxvtxzj%2FTRJqKJ8mCORf%2Ft7YLPdRh%2FY2uBRR8%2B8Gs%2B%2FqnsVS9z4ejioTroY8eJ%2FiU5bGSsylb9hieW8H76&X-Amz-Signature=3b447954e074827f7d96cc6ac8f6620663163886ef83180da2f4b01ff2e862ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

