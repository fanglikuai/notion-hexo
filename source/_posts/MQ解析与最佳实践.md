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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PGRT5I7%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH6y0JYeE2wIxIFUwk1c9ktqse9y%2BE1woxrDSYRbu6JmAiEAhYAWgA7uUMMFJ6viZThowpJF8sr8hE8YOCgOjBFX%2FNIq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDEdJKeE31hj0NfOsfSrcAyQVhlkJlmAmOM%2BHgx%2BqAAuUeR4BlQI%2F9KJ6qqnBmQJOYPdCZQx9cN3v5QO7DFj22kMThi4GsL%2BTUgzxFj4XvnLK5oZLgnt22LKhapFvYsNRC%2BwbOIgvWyGM8bW9sTJtYRuNMeeVSTkj9HtZil1JxYbR34KFpSOAbL6gevEgTQp6cp7umH6Kvz2G2rEQYNSnZOpBBNP2nEZBH2S0K%2FWlsKhzxm9l615GvuVgr%2FUJ2VkJ58F9iMEJRfAARwxSJbnfPgUYBEat%2BvknwrvhC3PiSUwAnH77ITr3a3TsH72ZH0uegUpbyiWnV4YojlYbpekAjXx%2BDzE7fe3Wbg9AmYXpuD7KhiZQO2IXu1SnKa7S2xMKcqKT41rnECt2NUhbWaryStG%2BShhs0Xkmxb7jL6bEUoCkVMlJzYT2X9Tb16RqGrpcXd%2B7boqiq0UAm4StffrS5so2J9%2B4QQcJ15yqadahwBvAwoH1HcB2w5pKPHiGFtl3ZtQZjNNyePUhL88bfAs0lmL8xWO1i%2Fsl1lyLL8FtBiouMCx9I2a3Nmar4Q%2BxIKEjar%2BZrP8oYkwe9wWUDICmk2Wpy6ZPhBr%2BqCDrn%2BoCpjgJEpTo6l83%2F7WAd2TnO5JJH5dV0VdLUtRUGB9NMKfhg8cGOqUBweaSpVrynROPZ%2BcXkITv5f10r6YS4I44bigaFtS2hQHPXPxMH5n94xIpWFdnU1eaQHUe7SOLn89J5etyoGHDDTriohRBnQyPcKxSTspr%2Bc4BF8dZfZHHlHl9rZPsSj43u04J%2BeNaKbRkrB8JR1wUhAExgOAqp0TuACsxfDZnJBUyg2JrjBfimU0SyK%2FCAgBGd4OR5aQgCvfvTNALxoF0tVkvJtYm&X-Amz-Signature=3ebb9f391b645e46534a612772d9e329b3166dfb4e3ab53958b28fad7ca7be52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

