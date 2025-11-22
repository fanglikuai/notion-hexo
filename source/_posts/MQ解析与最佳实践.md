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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PKZXDGC%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIErjMkwTodcjcIXg3HEaiL8BY9QLIR3MhK4hGuyr1BU0AiAKXNuAwuHZCKFFqHqr0TKtkY1gsCO%2FXS8N9mAh%2BfwSBSr%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMGcLHRHto5OSRF4tGKtwD3WEdk60Hk%2BFCcm21KgHgL4x2dYTMPNxixFe2zXlK3Cu7RULwo8s6ZSgA%2FxV7iN8nnz64tQf7fsavUg8JZT3DOVCapGaLSo3d8HEpjX5zsUpd0Jeg75Eo7vvqQhRKfxnpMUc1uHkBc3JsdGf0nfhRtWZWQjcyaXZKv7QWPSZ%2BH2ZVza0hlhlfk6Qo6%2Fox7Zcz8VDAwwd3rNfiAyqrs1Hr2LxizFrrRNic8%2FoFYF51vGK3BEU91iOHSpGv6tDsWpSxu8CL3Fxs1sBVISED6JJNpNyBBPvLo4LiNIWR0FICeryFYFlhPZ89h1%2FwAY9uCp6AbMGexqaGdaUclNwUT%2B8mjsFPjulX94XvJ8Oh4bjtKDnKfTJP7DzyzXSVzlIvqUVF4ywEnJe7HfCI5RpO%2F%2BSEyyEpgJ3ZUPuHLV3Ll43Uwln4caYbcVFtpWSn9IVQTsz8bLJKASVJAXi7O0GnFbbJ9Q8VtCleAOjwLp%2FJLaJQT0B0IGG8WGOwyWvRSmmgChbAOuT3%2BZo1%2FfqkNHvAiRzXeOuzpvDtT5e8b3AbpCKpayFHMCrahdei%2BFH%2FCQbYtaqQbVLhy%2B1EULJP6V9fB4t5UgtRNWkHuMUuHbWze5DZYjfAsqXvxdDntvhM2HAwiaKGyQY6pgFP3aG6H4H6i3t4VHXkXIQVZnr8Ogb0rdK4ogzkrsu6q%2FcyS4%2FtxnLey8iYhBQNTfC8beX5losltAynXWlHuwuUTtYpGyskxd4uiSOyPlUAIvKXw1FavptLaqJ4C%2FM0kkmiyM7mGymISKVkWkhGa%2FAxUwyBqmGUZd3ORwtDQLlrPRZN5zhRg5VEpjpeF8HLYgJpm8hcNT9vqU62uf9DqXNuBMa81ab8&X-Amz-Signature=53232917bc9339cd63cbbd2edef8aedaacd585425b075afce15b080e1623c00d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

