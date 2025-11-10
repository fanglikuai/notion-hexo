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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6KTGNK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDiLY66IWlisOWeT4QbbJHingk3eoJr1EZBfbxLzip9wQIgLZ0UnHo%2BDvdi3EChGS86Oj%2FnhvTwUTW5hUJeCBHThpYqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDut71d9KhxvhTKsRCrcA9Ty5ZHJ5AyOIpk3k8tjezu3OZCaY1KQqc9NgwJCeJmlGZm9Q7qFc%2Fje8zzY%2FNqvzSBvxdNjAmrLQYmQjHa7SxXZUqrX3oVzEO9JMF%2FqFBxqGRrybhGBALSukexQxq%2FCTZGiBhqLFoHaFxIiWXLJHjZdBORF7vd%2FHmg68341C2ol%2FGHttsaH7X9nbUZHGl5CBEhZ%2Bi1bxtA2sNwoKPDzbIoJzlNdDLzDZMxPuH6NzHjxmT1EfMs4bzrC5DZAnW1v8jVsahWjjcyflArBWJHLVWch89TuU%2BGXH4vN6UDp0WWKWeSQJhIA%2BL8zxS4Pm4yUGslukhlH1JCOaoeWLpIGopWN1zilU%2FU67K6xEveLRR0GHZ6U8rD4pIPw2lqnkBExJvaImBQtdMCMkhCWj01g4noKroWZOE7MHRrmJpE9xUGoD2acWgIzv3%2BifTtg1yfSEEWvx%2Fsqe7EcFCUusPJuD5xW5WB7RMcxZxbk5et%2F59uwNYNPUYF70Q4TpBuCJIOnGIBdO0MfjyE9Gb9QHxNHBFcPHri6ODLBJ%2BKVxN2mwl07zUWhp12NmV8ef41%2FM4MYb0%2FZdNhcchynsUmY%2Ff8b9dS0fSxi%2FvssNjEsuFWxVzH1ISovBKOgxbIbQGdFMPX2xcgGOqUB88TT5CgcIu4sUGa9cu1YBL68udJ%2FNzLzTuthJoztgMjvE4GqrB%2FnuDXbOMxaaI3xwXQkDbif354v1VdTFrgT2n1JF4zFKNiMDpeYzFBiICPpr5OE3eI3yGYX3tMxpvqCwTQl0uWMfuop%2B%2FbnUN7lTqvb0iUPZO0wGcSQ0N7z9A3cSOPCNGkP07OLrmUpwMokUpWWMZWQan8ge5mjD8aZn1OWdNJ2&X-Amz-Signature=2aa63e347c047b82bfe1cda934b55c9d6a14b7c4de5aaf4361653ca8e6fae398&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

